---
title: Export process and paperless trade
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
# Update customs data

Once you're shipping to non domestic destinations, the carrier might require customs related data to handle clearance. You can use the method UPDATE_CUSTOMS_DATA of class /AEB/CL_PA_PB_CARRIER_BF to update customs data for an existing shipping order. Updates can be done on header and item level.

Required data includes certain reference numbers (MRN), item values and others. See [API - Update customs data ](https://transport-freight-management.docs.developers.aeb.com/reference/updatecustomsdata) for a documentation of the available fields for an update. Here is a coding example:

<br />

```text Update customs data
"========================================================================================================================
"Update Customs data

DATA: lo_update_customs_data_request TYPE REF TO /aeb/cl_pa_pb_upd_cust_req_do,
      ls_likp                        TYPE likp,
      lt_lips                        TYPE /aeb/01_lipss,
      lo_reference_text              TYPE REF TO /aeb/cl_pa_pb_ref_text_ucd_do,
      lt_reference_texts             TYPE /aeb/cl_pa_pb_ref_text_ucd_do=>tt_ref_text_ucd_do,
      lv_mrn                         TYPE /aeb/01_char500,
      lt_langs                       TYPE /aeb/01_char2s,
      lv_lang                        TYPE /aeb/01_char2 VALUE 'DE'
      .
      
"Select the delievery data - adjust the criteria accordingly for your process
SELECT SINGLE * FROM likp WHERE vbeln = '0080000000' INTO @ls_likp.
SELECT * FROM lips WHERE vbeln = '0080000000' INTO TABLE @lt_lips.

"Gather parameters for the connection to Carrier Connect 
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
"Reference of the shipping order 
DATA(lo_reference_bc) = /aeb/cl_pa_pb_dlv_shp_ref_bc=>new_for( im_likp = ls_likp ).
DATA(lv_shp_ref) = lo_reference_bc->get_shp_ref_do( ).
APPEND lv_lang TO lt_langs.

CREATE OBJECT lo_update_customs_data_request.
lo_update_customs_data_request->set_client_identcode( im_value = lo_engn_prm->get_engine_client( ) ).
lo_update_customs_data_request->set_client_system_id( im_value = sy-sysid && '_' && sy-mandt ).

"Additional fields might be required to sent carrier specific values
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
lv_mrn = '1234567890'.
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
"End of update Customs data
"======================================================================================================================== 

```

<br />

# Customs document and invoice document

"Paperless trade" usually also requires the upload of certain documents provided by customs, e.g. an export accompaning document (EAD). For exports from Germany , its called ABD.

In addition, the invoice document from SAP is often required. See <Anchor target="_blank" href="https://sap-plugins.docs.developers.aeb.com/docs/attach-documents-to-a-shipping-order">Attach documents</Anchor>for more infomation how to upload a document to Carrier Connect from SAP.
