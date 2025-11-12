---
title: Further functions
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Public Class /AEB/CL_TA_PB_MATERIAL_BF

You can use the method REQUEST_CLASSIFICATION of this class for reading classifications directly from Product Classification or, depending on the settings there,  force the creation of classification values. 

This method can also be used to set the priority and reference of classifications in the engine (if the according classification is not yet approved completely). For details, see [Request Classification](https://customsmanagement.docs.developers.aeb.com/docs/request-the-classification-data-for-a-profile)

```Text Set priority and reference
DATA:
  pb_bf         TYPE REF TO /aeb/cl_ta_pb_material_bf,
  result        TYPE REF TO /aeb/cl_ta_pb_req_cls_res_do,
  exception     TYPE REF TO /aeb/cx_01_pb_bf_inv_sc,
  error_message TYPE string.

pb_bf = /aeb/cl_ta_pb_material_bf=>new_for( 'DEFAULT' ). "Organisational unit

TRY .
    result = pb_bf->request_classification(
        im_material_no                = 'M-11'
        im_profile_code               = 'DE'
        im_priority                   = '3'
        im_reference                  = 'Reference' ).
  CATCH /aeb/cx_01_pb_bf_inv_sc INTO exception.
    error_message = exception->/aeb/if_01_cx_message~get_msg_as_str( ).
ENDTRY.
```