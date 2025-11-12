---
title: Send data to ASSIST4
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
To send data to the ASSIST4 system you need a engine which actually makes the request. To create the engine you can pass the ID of the system you want to send data to, to the factory class /AEB/CL\_CT\_PB\_AS4DOC\_XXX\_FA. Then call the method SET\_CUS\_DATA and pass it the business facade name and the data.\
The business facade name is made up of the literal 'As4Doc' and the external Name of the document (found in the definition) in the ASSIST4 system separated by underscore.

```text Send data
DATA:
  data        TYPE ts_parameter,
  engn        TYPE REF TO /aeb/if_ct_pb_as4doc_xxx_bf,
  result      TYPE REF TO /aeb/if_ct_pb_nsg_doc_res_dto.

"...
"fill data
"...

engn = /aeb/cl_ct_pb_as4doc_xxx_fa=>as4doc_xxx( 'SYSTEM_ID' ).
result = engn->set_cus_data( im_bf_name  = 'As4Doc_BF_NAME' "Please replace with your BF Name
                             im_cus_data = data ).
```
```text Full example
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
  data        TYPE ts_parameter,
  engn        TYPE REF TO /aeb/if_ct_pb_as4doc_xxx_bf,
  result      TYPE REF TO /aeb/if_ct_pb_nsg_doc_res_dto.

"...
"fill data
"...

engn = /aeb/cl_ct_pb_as4doc_xxx_fa=>as4doc_xxx( 'SYSTEM_ID' ).
result = engn->set_cus_data( im_bf_name  = 'As4Doc_BF_NAME' "Please replace with your BF Name
                             im_cus_data = data ).
```
