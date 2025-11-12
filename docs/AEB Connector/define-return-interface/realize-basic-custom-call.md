---
title: Realize basic custom call
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Now that you have defined the structure of the result from the ASSIST4 system, you can create the basic call to receive the data in your defined structure. The variable as4data then contains the data and can be used for your specific workflow.\
You trigger the basic call from BAdI /AEB/CT\_NSG\_WRK\_01.  Check the basic and the full example below.

```text Basic call
DATA:
  bo_id               TYPE string,
  result_messages_dto TYPE REF TO /aeb/if_ct_pb_nsg_doc_res_dto,
  as4data             TYPE ts_parameter.

bo_id = im_journal_entry_group->get_bo_id( ).

TRY.
  im_engine_if->get_cus_data( EXPORTING
                                im_bf_name = 'As4Doc_BF_NAME' "Please replace with your BF Name
                                im_businessobjectid = bo_id
                              IMPORTING
                                ex_result_dto = result_messages_dto
                              CHANGING
                                ch_result_data_target = as4data ).
  CATCH /aeb/cx_01_pb_missing_parm_sc.
ENDTRY.
```
```text Full Example
TYPES: BEGIN OF ts_as41cadfld,
         fld_name  TYPE string,
         fld_value TYPE string,
       END OF ts_as41cadfld.
TYPES tt_as41cadfld TYPE STANDARD TABLE OF ts_as41cadfld WITH DEFAULT KEY.
TYPES: BEGIN OF ts_ae41cons,
         cons_no      TYPE string,
         client_cde   TYPE string,
         despatchdate TYPE string,
         as41cadfld   TYPE tt_as41cadfld,
       END OF ts_ae41cons.
TYPES tt_ae41cons TYPE STANDARD TABLE OF ts_ae41cons WITH DEFAULT KEY.
TYPES: BEGIN OF ts_parameter,
         ae41cons TYPE tt_ae41cons,
       END OF ts_parameter.
       
DATA:
  bo_id               TYPE string,
  result_messages_dto TYPE REF TO /aeb/if_ct_pb_nsg_doc_res_dto,
  as4data             TYPE ts_parameter.

bo_id = im_journal_entry_group->get_bo_id( ).

TRY.
  im_engine_if->get_cus_data( EXPORTING
                                im_bf_name = 'As4Doc_BF_NAME' "Please replace with your BF Name
                                im_businessobjectid = bo_id
                              IMPORTING
                                ex_result_dto = result_messages_dto
                              CHANGING
                                ch_result_data_target = as4data ).
  CATCH /aeb/cx_01_pb_missing_parm_sc.
ENDTRY.
```

The BF-Name is made up of the literal 'As4Doc' and the external Name of the document (found in the definition) in the ASSIST4 system separated by underscore. Example: the document in ASSIST4 to sent back the data has the external name DELIVBACK. The BF name is As4Doc\_DELIVBACK
