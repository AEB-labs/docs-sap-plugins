---
title: Update shipping order (process shipment)
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
Use the method PROCESS_SHIPMENT of class /AEB/CL_PA_PB_CARRIER_BF for updates of a shipping order, e.g. adding packages to it. Example program:   

```text Process shipment
REPORT zaeb_process_shipment.

PARAMETERS: vbeln TYPE likp-vbeln.

DATA:
  likp                      TYPE likp,
  lipss                     TYPE STANDARD TABLE OF lips,
  org_unit_rule_bc          TYPE REF TO /aeb/cl_pa_pb_dlv_ou_rule_bc,
  org_unit                  TYPE /aeb/01_char20,
  engn_prm_bc               TYPE REF TO /aeb/cl_pa_pb_engn_prm_bc,
  engn_prm_mo               TYPE REF TO /aeb/if_pa_pb_engn_prm_mo,
  shp_ref_bc                TYPE REF TO /aeb/cl_pa_pb_dlv_shp_ref_bc,
  shipment_ref              TYPE /aeb/pa_pb_dl_shp_ref_do,
  carrier_bf                TYPE REF TO /aeb/cl_pa_pb_carrier_bf,
  request                   TYPE REF TO /aeb/cl_pa_pb_pr_shp_req_do,
  result_language_iso_codes TYPE /aeb/01_char2s,
  result                    TYPE REF TO /aeb/cl_pa_pb_pr_shp_res_do,
  exc                       TYPE REF TO /aeb/cx_01_pb_bf_inv_sc,
  msg                       TYPE REF TO /aeb/cl_01_pb_res_msg_do,
  text                      TYPE REF TO /aeb/cl_01_pb_til_do,
  process_parms             TYPE REF TO /aeb/cl_pa_pb_shp_prp_do,
  output_mode               TYPE REF TO /aeb/cl_pa_pb_doc_outm_do,
  doc_prepare_scope         TYPE REF TO /aeb/cl_pa_pb_doc_scope_do,
  output_scope              TYPE REF TO /aeb/cl_pa_pb_doc_scope_do,
  workstation_id            TYPE string,
  dlv_shp_coll_bc           TYPE REF TO /aeb/cl_pa_pb_dlv_shp_coll_bc,
  vepos                     TYPE /aeb/01_vepos,
  vekps                     TYPE /aeb/01_vekps,
  shp_req_do                TYPE /aeb/pa_pb_dl_shp_req_do.

"Read data for the outbound delivery  
SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.
SELECT * FROM lips INTO TABLE lipss WHERE vbeln = vbeln.

"Determine the packages (handling units) that you want to pass in the update request: 
"- e.g. by using CALL FUNCTION 'HU_PER_DOCUMENT_READ' 
"- or by reading the table VEKP  
"- or by any other sufficient method  
"Dont forget to remove packages that should not be updated 
"Eventually, assign the packages to variable vekps.   
"If required, read the packed items and assign them to variable vepos. 

"Organizational unit and connection 
org_unit_rule_bc = /aeb/cl_pa_pb_dlv_ou_rule_bc=>new_for( im_likp = likp im_lipss = lipss ).
org_unit = org_unit_rule_bc->get_org_unit( ).
engn_prm_bc = /aeb/cl_pa_pb_engn_prm_bc=>get_instance( ).
engn_prm_mo = engn_prm_bc->get_engn_prm_do_for( org_unit ).

"Reference of the shipment that will be updated 
shp_ref_bc = /aeb/cl_pa_pb_dlv_shp_ref_bc=>new_for( likp ).
shipment_ref = shp_ref_bc->get_shp_ref_do( ).

"Build the request structure
CREATE OBJECT request.
"General data
request->set_client_identcode( engn_prm_mo->get_engine_client( ) ).
request->set_client_system_id( sy-sysid && '_' && sy-mandt ).
request->set_username( sy-uname ).
APPEND 'DE' TO result_language_iso_codes.
request->set_result_language_iso_codes( result_language_iso_codes ).
request->set_shipment_reference( shipment_ref ).

"Start: Packages  
"(Optional) let the add-on collect the data.     
dlv_shp_coll_bc = /aeb/cl_pa_pb_dlv_shp_coll_bc=>new_for(
                    im_likp  = likp
                    im_vekps = vekps
                    im_vepos = vepos ).
shp_req_do = dlv_shp_coll_bc->create_shipment( im_org_unit = org_unit ).
"Add the packages to the request structure   
request->set_packages( im_value = shp_req_do-packages ).
"End: Packages 


"Start: Process parameters for document creation
"Documentation of the parameters: 
"https://transport-freight-management.docs.developers.aeb.com/docs/process-parameters-v2 

CREATE OBJECT process_parms.
process_parms->set_do_completion( '-' )."set value to X if you want to close the shipment 
process_parms->set_workstation_id( workstation_id ).
CREATE OBJECT output_mode.
output_mode->set_mode( 'RETURN' ).
process_parms->set_document_output_mode( output_mode ).
CREATE OBJECT doc_prepare_scope.
doc_prepare_scope->set_scope( 'REQUEST' ).
process_parms->set_document_prepare_scope( doc_prepare_scope ).
CREATE OBJECT output_scope.
output_scope->set_scope( 'REQUEST' ).
process_parms->set_document_output_scope( output_scope ).
request->set_process_parms( process_parms ).
"End: Process parameters for document creation 

TRY.
    carrier_bf = /aeb/cl_pa_pb_carrier_bf=>new_for( im_engn_prm = engn_prm_mo
                                                    im_org_unit = org_unit ).

    result = carrier_bf->process_shipment( request ).
    LOOP AT result->get_messages( ) INTO msg.
      LOOP AT msg->get_message_texts( ) INTO text.
        WRITE text->get_text( ).
        WRITE /.
      ENDLOOP.
    ENDLOOP.
  CATCH /aeb/cx_01_pb_bf_inv_sc INTO exc.
    WRITE exc->/aeb/if_01_cx_message~get_msg_as_str( ).
ENDTRY.
```