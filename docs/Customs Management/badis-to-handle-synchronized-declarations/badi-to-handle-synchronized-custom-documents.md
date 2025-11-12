---
title: BAdI to handle synchronized custom documents
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
The BAdI /AEB/AES_DOC_01 will be run through during the synchronization of the data. You can implement it to handle processing of received customs documents, e.g. EAD or POE. The documents are provided as a byte stream representing the PDF.  
The BAdI is run through only once a document is received the first time.

The following example is sending documents to Carrier Connect for Paperless Trade (e.g. DHL) for export declarations that are created from invoice and if the Carrier Connect shipping order is created from the delivery. 

```
METHOD /aeb/if_ex_aes_doc_01~document_received.

  IF im_type_of_received_doc <> 'ABD'.
    RETURN.
  ENDIF.

  DATA: lv_vbeln                       TYPE vbeln,
        ls_comwa                       TYPE vbco6,
        lt_vbfas                       TYPE TABLE OF vbfa,
        ls_likp                        TYPE likp,
        lt_lips                        TYPE /aeb/01_lipss,
        lo_request                     TYPE REF TO /aeb/cl_pa_pb_add_satt_req_do,
        lt_attachements                TYPE /aeb/cl_pa_pb_attachment_do=>tt_attachment_do,
        lo_attachement                 TYPE REF TO /aeb/cl_pa_pb_attachment_do,
        lt_langs                       TYPE /aeb/01_char2s,
        lv_lang                        TYPE /aeb/01_char2 VALUE 'DE',
        lo_process_request             TYPE REF TO /aeb/cl_pa_pb_pr_shp_req_do,
        lo_creation_parms              TYPE REF TO /aeb/cl_pa_pb_cr_prm_do,
        lo_update_data                 TYPE REF TO /aeb/cl_pa_pb_shp_upd_req_do,
        lv_mrn                         TYPE /aeb/01_char500,
        lo_update_customs_data_request TYPE REF TO /aeb/cl_pa_pb_upd_cust_req_do,
        lo_reference_text              TYPE REF TO /aeb/cl_pa_pb_ref_text_ucd_do,
        lt_reference_texts             TYPE /aeb/cl_pa_pb_ref_text_ucd_do=>tt_ref_text_ucd_do
        .

  "Read order flow to determine delivery number
  CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
    EXPORTING
      input  = im_transaction_label_host
    IMPORTING
      output = lv_vbeln.

  ls_comwa-vbeln = lv_vbeln.

  CALL FUNCTION 'RV_ORDER_FLOW_INFORMATION'
    EXPORTING
      comwa         = ls_comwa
    TABLES
      vbfa_tab      = lt_vbfas
    EXCEPTIONS
      no_vbfa       = 1
      no_vbuk_found = 2
      OTHERS        = 3.
  SORT lt_vbfas BY erdat DESCENDING erzet DESCENDING.

  READ TABLE lt_vbfas INTO DATA(ls_delivey) WITH KEY vbtyp_v = 'C'.
  SELECT SINGLE * FROM likp WHERE vbeln = @ls_delivey-vbeln INTO @ls_likp.
  SELECT * FROM lips WHERE vbeln = @ls_delivey-vbeln INTO TABLE @lt_lips.

  "Create add_shipment_attachments call to send ABD to CCO
  DATA(lo_org_unit_bc) = /aeb/cl_pa_pb_dlv_ou_rule_bc=>new_for(
                           im_likp  = ls_likp
                           im_lipss = lt_lips
                         ).
  DATA(lv_org_unit) = lo_org_unit_bc->get_org_unit( ).

  DATA(lo_connection_parms_bc) = /aeb/cl_pa_pb_engn_prm_bc=>get_instance( ).
  DATA(lo_engn_prm) = lo_connection_parms_bc->get_engn_prm_do_for( im_org_unit = lv_org_unit ).

  DATA(lo_carrier_bf) = /aeb/cl_pa_pb_carrier_bf=>new_for(
                          im_engn_prm = lo_engn_prm
                          im_org_unit = lv_org_unit
                        ).

  DATA(lo_reference_bc) = /aeb/cl_pa_pb_dlv_shp_ref_bc=>new_for( im_likp = ls_likp ).
  DATA(lv_shp_ref) = lo_reference_bc->get_shp_ref_do( ).

  CREATE OBJECT lo_attachement.
  lo_attachement->set_data( im_value = im_pdf_binary_of_received_doc ).
  lo_attachement->set_file_name( im_value = 'ABD_' && im_et_id && '.pdf' ).
  lo_attachement->set_content_type( im_value = 'EXPORT_ACCOMPANYING_DOCUMENT' ). "COMMERCIAL_INVOICE or PRO_FORMA_INVOICE
  lo_attachement->set_mime_type( im_value = 'application/pdf' ).

  APPEND lo_attachement TO lt_attachements.

  CREATE OBJECT lo_request.
  lo_request->set_client_identcode( im_value = lo_engn_prm->get_engine_client( ) ).
  lo_request->set_client_system_id( im_value = sy-sysid && '_' && sy-mandt ).
  APPEND lv_lang TO lt_langs.
  lo_request->set_result_language_iso_codes( im_value = lt_langs ).
  lo_request->set_mode( im_value = 'ADD' ).
  lo_request->set_shipment_reference( im_value = lv_shp_ref ).
  lo_request->set_attachments( im_value = lt_attachements ).
  lo_request->set_username( im_value = sy-uname ).

  TRY.
      DATA(lo_result) = lo_carrier_bf->add_shipment_attachments( im_request_do = lo_request ).
    CATCH /aeb/cx_01_pb_bf_inv_sc. " Communication exception
      "Error handling here
  ENDTRY.

  "Update customs data
  CREATE OBJECT lo_update_customs_data_request.
  lo_update_customs_data_request->set_client_identcode( im_value = lo_engn_prm->get_engine_client( ) ).
  lo_update_customs_data_request->set_client_system_id( im_value = sy-sysid && '_' && sy-mandt ).
  "Necessary for carrier individuel values
*    lo_add_fields_record = lo_data_object_factory->new_01_gen_data_rec_do( ).
*    lo_add_field = lo_data_object_factory->new_01_gen_data_field_do(
*                     im_name  =
*                     im_type  =
*                     im_value =
*                   ).
*    lo_add_fields_record->set_fields( im_value = lt_add_fields ).
*    lo_update_customs_data_request->set_additional_values( im_value = lo_add_fields_record ).
  CREATE OBJECT lo_reference_text.
  lo_reference_text->set_type( im_value = 'CUSTOMS_REGISTRATION_NUMBER' ).
  lv_mrn = im_declaration->get_customs_registration_numbe( )->v.
  lo_reference_text->set_value( im_value = lv_mrn ).
  APPEND lo_reference_text TO lt_reference_texts.
  lo_update_customs_data_request->set_customs_references_texts( im_value = lt_reference_texts ).
  lo_update_customs_data_request->set_result_language_iso_codes( im_value = lt_langs ).
  lo_update_customs_data_request->set_shipment_reference( im_value = lv_shp_ref ).
  lo_update_customs_data_request->set_username( im_value = sy-uname ).

  TRY.
      DATA(lo_update_result) = lo_carrier_bf->update_customs_data( im_request = lo_update_customs_data_request ).
    CATCH /aeb/cx_01_pb_bf_inv_sc. " Communication exception
      "Error handling here
  ENDTRY.


ENDMETHOD.
```

Example to print a document directly from the badi

```
DATA: lv_spool_handle TYPE syst_tabix,  
          lv_spool        TYPE rspoid,  
          lv_xstring      TYPE xstring,  
          lv_filesize     TYPE i,  
          lt_raw_buffer   TYPE solix_tab,  
          lv_title        TYPE rspotitle,  
          lv_name         TYPE rspo0name,  
          lv_suffix1      TYPE rspo1name,  
          lv_suffix2      TYPE rspo2name,  
          lv_ldest        TYPE ldest,  
          lo_logger       TYPE REF TO /aeb/cl_aes_pb_logger_bc.

"Only print ABD
IF im_type_of_received_doc <> 'ABD'.
  RETURN.
ENDIF.

lo_logger = /aeb/cl_aes_pb_logger_bc=>new( ).

" Prepare data for output of the labels
lv_xstring    = im_pdf_binary_of_received_doc.
lt_raw_buffer = cl_bcs_convert=>xstring_to_solix( iv_xstring = lv_xstring ).
lv_filesize   = xstrlen( lv_xstring ).

" Spool-Settings
CONCATENATE 'ABD' im_reference_no_aes INTO lv_title SEPARATED BY space.
lv_name    = 'AEB'.
lv_suffix2 = im_reference_no_aes.
lv_ldest   = 'PDF'.  "Printer has to be determined beforehand


IF lv_ldest IS INITIAL.
  " No printer determined for output based in AEB-parameter!
  lo_logger->create_log_error(
    EXPORTING
      im_org_unit = CONV #( im_declaration->get_org_unit( )->v )       " Organizational unit
      im_ref_no   = CONV #( im_reference_no_aes )                " Reference
      im_log_text = 'No printer determined for output based in AEB-parameter!'                 " Log text
  ).
  RETURN.
ENDIF.

" Create Spool Request
CALL FUNCTION 'RSPO_SR_OPEN'
  EXPORTING
    dest             = lv_ldest
    layout           = 'G_RAW'
    doctype          = 'BIN'
    titleline        = lv_title
    suffix1          = lv_suffix1
    suffix2          = lv_suffix2
    name             = lv_name
    immediate_print  = abap_true       " Direct Print
  IMPORTING
    handle           = lv_spool_handle
    spoolid          = lv_spool
  EXCEPTIONS
    device_missing   = 1
    name_twice       = 2
    no_such_device   = 3
    operation_failed = 4
    OTHERS           = 5.

IF sy-subrc NE 0.
  lo_logger->create_log_error(
    EXPORTING
      im_org_unit = CONV #( im_declaration->get_org_unit( )->v )       " Organizational unit
      im_ref_no   = CONV #( im_reference_no_aes )                " Reference
      im_log_text = 'Error in creating the spool requests'                 " Log text
  ).
  RETURN.
ENDIF.

" Add the binary content into Spool Request
CALL FUNCTION 'RSPO_SR_TABLE_WRITE_BINARY'
  EXPORTING
    handle           = lv_spool_handle
    total            = lv_filesize
  TABLES
    lines            = lt_raw_buffer
  EXCEPTIONS
    handle_not_valid = 1.

IF sy-subrc NE 0.
  lo_logger->create_log_error(
    EXPORTING
      im_org_unit = CONV #( im_declaration->get_org_unit( )->v )       " Organizational unit
      im_ref_no   = CONV #( im_reference_no_aes )                " Reference
      im_log_text = 'Error adding the binary content into spool request'                 " Log text
  ).
  RETURN.
ENDIF.

" Close Spool Request
CALL FUNCTION 'RSPO_SR_CLOSE'
  EXPORTING
    handle           = lv_spool_handle
  EXCEPTIONS
    handle_not_valid = 1
    operation_failed = 2
    OTHERS           = 3.

IF sy-subrc NE 0.
  lo_logger->create_log_error(
    EXPORTING
      im_org_unit = CONV #( im_declaration->get_org_unit( )->v )       " Organizational unit
      im_ref_no   = CONV #( im_reference_no_aes )                " Reference
      im_log_text = 'Error closing spool request'                 " Log text
  ).
  RETURN.
ENDIF.
```