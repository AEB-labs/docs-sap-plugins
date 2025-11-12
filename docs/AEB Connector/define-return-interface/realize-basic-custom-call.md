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
Now that you have defined the structure of the result from the ASSIST4 system, you can create the basic call to receive the data in your defined structure. The variable as4data then contains the data and can be used for your specific workflow.
You trigger the basic call from BAdI /AEB/CT_NSG_WRK_01.  Check the basic and the full example below.
[block:code]
{
  "codes": [
    {
      "code": "DATA:\n  bo_id               TYPE string,\n  result_messages_dto TYPE REF TO /aeb/if_ct_pb_nsg_doc_res_dto,\n  as4data             TYPE ts_parameter.\n\nbo_id = im_journal_entry_group->get_bo_id( ).\n\nTRY.\n  im_engine_if->get_cus_data( EXPORTING\n                                im_bf_name = 'As4Doc_BF_NAME' \"Please replace with your BF Name\n                                im_businessobjectid = bo_id\n                              IMPORTING\n                                ex_result_dto = result_messages_dto\n                              CHANGING\n                                ch_result_data_target = as4data ).\n  CATCH /aeb/cx_01_pb_missing_parm_sc.\nENDTRY.",
      "language": "text",
      "name": "Basic call"
    },
    {
      "code": "TYPES: BEGIN OF ts_as41cadfld,\n         fld_name  TYPE string,\n         fld_value TYPE string,\n       END OF ts_as41cadfld.\nTYPES tt_as41cadfld TYPE STANDARD TABLE OF ts_as41cadfld WITH DEFAULT KEY.\nTYPES: BEGIN OF ts_ae41cons,\n         cons_no      TYPE string,\n         client_cde   TYPE string,\n         despatchdate TYPE string,\n         as41cadfld   TYPE tt_as41cadfld,\n       END OF ts_ae41cons.\nTYPES tt_ae41cons TYPE STANDARD TABLE OF ts_ae41cons WITH DEFAULT KEY.\nTYPES: BEGIN OF ts_parameter,\n         ae41cons TYPE tt_ae41cons,\n       END OF ts_parameter.\n       \nDATA:\n  bo_id               TYPE string,\n  result_messages_dto TYPE REF TO /aeb/if_ct_pb_nsg_doc_res_dto,\n  as4data             TYPE ts_parameter.\n\nbo_id = im_journal_entry_group->get_bo_id( ).\n\nTRY.\n  im_engine_if->get_cus_data( EXPORTING\n                                im_bf_name = 'As4Doc_BF_NAME' \"Please replace with your BF Name\n                                im_businessobjectid = bo_id\n                              IMPORTING\n                                ex_result_dto = result_messages_dto\n                              CHANGING\n                                ch_result_data_target = as4data ).\n  CATCH /aeb/cx_01_pb_missing_parm_sc.\nENDTRY.",
      "language": "text",
      "name": "Full Example"
    }
  ]
}
[/block]
The BF-Name is made up of the literal 'As4Doc' and the external Name of the document (found in the definition) in the ASSIST4 system separated by underscore. Example: the document in ASSIST4 to sent back the data has the external name DELIVBACK. The BF name is As4Doc_DELIVBACK