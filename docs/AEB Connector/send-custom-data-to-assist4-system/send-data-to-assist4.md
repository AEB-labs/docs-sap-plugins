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
To send data to the ASSIST4 system you need a engine which actually makes the request. To create the engine you can pass the ID of the system you want to send data to, to the factory class /AEB/CL_CT_PB_AS4DOC_XXX_FA. Then call the method SET_CUS_DATA and pass it the business facade name and the data.
The business facade name is made up of the literal 'As4Doc' and the external Name of the document (found in the definition) in the ASSIST4 system separated by underscore.
[block:code]
{
  "codes": [
    {
      "code": "DATA:\n  data        TYPE ts_parameter,\n  engn        TYPE REF TO /aeb/if_ct_pb_as4doc_xxx_bf,\n  result      TYPE REF TO /aeb/if_ct_pb_nsg_doc_res_dto.\n\n\"...\n\"fill data\n\"...\n\nengn = /aeb/cl_ct_pb_as4doc_xxx_fa=>as4doc_xxx( 'SYSTEM_ID' ).\nresult = engn->set_cus_data( im_bf_name  = 'As4Doc_BF_NAME' \"Please replace with your BF Name\n                             im_cus_data = data ).",
      "language": "text",
      "name": "Send data"
    },
    {
      "code": "TYPES: BEGIN OF ts_as41cadfld,\n         fld_name  TYPE string,\n         fld_value TYPE string,\n       END OF ts_as41cadfld.\nTYPES tt_as41cadfld TYPE STANDARD TABLE OF ts_as41cadfld WITH DEFAULT KEY.\nTYPES: BEGIN OF ts_ae41cons,\n         cons_no      TYPE string,\n         client_cde   TYPE string,\n         despatchdate TYPE string,\n         as41cadfld   TYPE tt_as41cadfld,\n       END OF ts_ae41cons.\nTYPES tt_ae41cons TYPE STANDARD TABLE OF ts_ae41cons WITH DEFAULT KEY.\nTYPES: BEGIN OF ts_parameter,\n         ae41cons TYPE tt_ae41cons,\n       END OF ts_parameter.\nDATA:\n  data        TYPE ts_parameter,\n  engn        TYPE REF TO /aeb/if_ct_pb_as4doc_xxx_bf,\n  result      TYPE REF TO /aeb/if_ct_pb_nsg_doc_res_dto.\n\n\"...\n\"fill data\n\"...\n\nengn = /aeb/cl_ct_pb_as4doc_xxx_fa=>as4doc_xxx( 'SYSTEM_ID' ).\nresult = engn->set_cus_data( im_bf_name  = 'As4Doc_BF_NAME' \"Please replace with your BF Name\n                             im_cus_data = data ).",
      "language": "text",
      "name": "Full example"
    }
  ]
}
[/block]