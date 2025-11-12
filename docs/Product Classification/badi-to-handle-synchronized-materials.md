---
title: BAdI to handle synchronized material data
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
The BAdI /AEB/TA_CLS_SYNC_01 is called every time a classification value is received from the Product Classification Engine, which is every time the classification value was changed. The BAdI /AEB/TA_CER_SYNC_01 is the same for the certificates. 

In both BAdIs you have a method that says that the data is updated (Inserted or updated) and one method if the data is deleted. 

Both BAdI supports the raising of the exception /aeb/cx_ta_pb_mat_sync_sc as it is described under <https://sap-plugins.docs.developers.aeb.com/docs/exceptions>. When the exception is raised, then the complete handling (also the standard handling) of the material is considered as failed, which means there will be an entry created in the function call monitor which can be automatically repeated with the report /AEB/01_FCC_RETRY if the customizing is done. When this repeat happens (either automatically by the report or manually by a user in the function call monitor) then the whole material is transferred from the Product Classification engine and processed and also this BAdI will be called again.

```text Handle classification value is updated/inserted
METHOD /aeb/if_ex_ta_cls_sync_01~hdl_classification_updated.
  WRITE im_je_cls_value_do->get_approved( ).
  WRITE im_je_cls_value_do->get_engn_client( ).
  WRITE im_je_cls_value_do->get_engn_dest( ).
  WRITE im_je_cls_value_do->get_material_no( ).
  WRITE im_je_cls_value_do->get_object_id( ).
  WRITE im_je_cls_value_do->get_processing_comment( ).
  WRITE im_je_cls_value_do->get_type_name( ).
  WRITE im_je_cls_value_do->get_valid_from( ).
  WRITE im_je_cls_value_do->get_valid_to( ).
  WRITE im_je_cls_value_do->get_value( ).

  RAISE EXCEPTION TYPE /aeb/cx_ta_pb_mat_sync_sc.
ENDMETHOD.
```
```Text Handle classification value - "today valid"
  METHOD /aeb/if_ex_ta_cls_sync_01~hdl_classification_updated.
    DATA:
      lv_matnr TYPE marc-matnr.

    IF im_je_cls_value_do->get_approved( ) <> 'X'.
      RETURN.
    ENDIF.

    " if synced value valid from is newer then today / in the future ..
    IF im_je_cls_value_do->get_valid_from( ) >= sy-datum.
      RETURN.
    ENDIF.

    " if synced value valid to is older then today / already exceeded ..
    IF im_je_cls_value_do->get_valid_to( ) IS INITIAL OR im_je_cls_value_do->get_valid_to( ) <= sy-datum.
      RETURN.
    ENDIF.

    lv_matnr = im_je_cls_value_do->get_material_no( ).
 		" IdentCodes are just examples, these need to be aligned with the identcode in your product classification system    
    CASE im_je_cls_value_do->get_type_name( ).
      WHEN 'EXPORT_CONTROL_DE'. " German Ausfluhrlistennummer
        " here change your SAP Data with im_je_cls_value_do->get_value( ).
     "when 'EXPORT_CONTROL_DE'. " EU Dual Use no
     "when 'COCO_EXPORT_DE'.
     "when 'COCO_EXPORT_DESC_DE'.
     "when 'COCO_IMPORT_DE'.
     "when 'COCO_IMPORT_DESC_DE'.
     "when 'EXPORT_CONTROL_US'. " US ECCN
    ENDCASE.
  ENDMETHOD.
```
```text Handle certificate value is udpated/inserted
METHOD /aeb/if_ex_ta_cer_sync_01~hdl_certificate_updated.
  WRITE im_je_certificate_do->get_cert_additional_expl( ).
  WRITE im_je_certificate_do->get_cert_date_of_issue( ).
  WRITE im_je_certificate_do->get_cert_detail( ).
  WRITE im_je_certificate_do->get_cert_end_of_validity( ).
  WRITE im_je_certificate_do->get_cert_reference( ).
  WRITE im_je_certificate_do->get_country_of_certificate( ).
  WRITE im_je_certificate_do->get_division( ).
  WRITE im_je_certificate_do->get_engn_client( ).
  WRITE im_je_certificate_do->get_engn_dest( ).
  WRITE im_je_certificate_do->get_geo_id( ). 
  WRITE im_je_certificate_do->get_material_no( ).
  WRITE im_je_certificate_do->get_object_id( ).
  WRITE im_je_certificate_do->get_type_code( ).
  WRITE im_je_certificate_do->get_type_qualifier( ).
  WRITE im_je_certificate_do->get_valid_from( ).
  write im_je_certificate_do->get_valid_to( ).
    
  RAISE EXCEPTION TYPE /aeb/cx_ta_pb_mat_sync_sc.
ENDMETHOD.
```