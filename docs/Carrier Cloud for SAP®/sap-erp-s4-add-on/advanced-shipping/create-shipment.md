---
title: Create shipping order (create shipment)
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
With the preliminary steps listed under [Advanced shipping](https://sap-plugins.docs.developers.aeb.com/docs/processing-more-in-detail)  you have collected and prepared required data.  By using this data and calling the method "create_shipment" of class /AEB/CL_PA_PB_CARRIER_BF  you can create a shipping order.

This example creates a shipping order for a delivery. If you want to transfer a different kind of business object, use the according classes for it (see list [Advanced shipping - supporting functions](https://sap-plugins.docs.developers.aeb.com/docs/processing-more-in-detail))

```text Create shipping order
REPORT zaeb_create_first_shipment.

PARAMETERS: im_vbeln TYPE likp-vbeln.

DATA:
  likp             TYPE likp,
  lipss            TYPE STANDARD TABLE OF lips,
  vbpas            TYPE STANDARD TABLE OF vbpa,
  vekps            TYPE STANDARD TABLE OF vekp,
	vepos            TYPE STANDARD TABLE OF vepo,
  org_unit_rule_bc TYPE REF TO /aeb/cl_pa_pb_dlv_ou_rule_bc,
  org_unit         TYPE /aeb/01_char20,
  engn_prm_bc      TYPE REF TO /aeb/cl_pa_pb_engn_prm_bc,
  engn_prm_mo      TYPE REF TO /aeb/if_pa_pb_engn_prm_mo,
  collector_bc     TYPE REF TO /aeb/cl_pa_pb_dlv_shp_coll_bc,
  shipment         TYPE /aeb/pa_pb_dl_shp_req_do,
  workstation_id   TYPE string,
  request                   TYPE REF TO /aeb/cl_pa_pb_cr_shp_req_do,
  result_language_iso_codes TYPE /aeb/01_char2s,
  result                    TYPE REF TO /aeb/cl_pa_pb_cr_shp_res_do,
  exc                       TYPE REF TO /aeb/cx_01_pb_bf_inv_sc,
  msg                       TYPE REF TO /aeb/cl_01_pb_res_msg_do,
  text                      TYPE REF TO /aeb/cl_01_pb_til_do,
  creation_parms            TYPE REF TO /aeb/cl_pa_pb_cr_prm_do,
  validation_parms          TYPE REF TO /aeb/cl_pa_pb_cr_shp_res_do,
  process_parms             TYPE REF TO /aeb/cl_pa_pb_shp_prp_do,
  output_mode               TYPE REF TO /aeb/cl_pa_pb_doc_outm_do,
  prepare_scope             TYPE REF TO /aeb/cl_pa_pb_doc_scope_do,
  process_mode              TYPE /aeb/pa_pb_dl_pmode_do,
  output_scope              TYPE REF TO /aeb/cl_pa_pb_doc_scope_do,
  carrier_bf                TYPE REF TO /aeb/cl_pa_pb_carrier_bf.

*------- Delivery header and related sub records  ----
SELECT SINGLE * FROM likp INTO likp WHERE vbeln = im_vbeln.
SELECT * FROM lips INTO TABLE lipss WHERE vbeln = im_vbeln.
SELECT * FROM vbpa INTO TABLE vbpas WHERE vbeln = im_vbeln.
"Determine vekps for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB
"Determine vepos for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB

*------- Organizational unit of the AEB add-on ----
org_unit_rule_bc = /aeb/cl_pa_pb_dlv_ou_rule_bc=>new_for( im_likp = likp im_lipss = lipss ).
org_unit = org_unit_rule_bc->get_org_unit( ).
WRITE: 'OrgUnit: ' , org_unit.
WRITE /.

*------- Connection to the end point at AEB cloud ----
engn_prm_bc = /aeb/cl_pa_pb_engn_prm_bc=>get_instance( ).
engn_prm_mo = engn_prm_bc->get_engn_prm_do_for( org_unit ).

WRITE: 'Destination: ' , engn_prm_mo->get_destination( ).
WRITE /.
WRITE: 'Client of Carrier Connect: ' , engn_prm_mo->get_engine_client( ).
WRITE /.

*------- Collect the data based on configured logic ----
collector_bc = /aeb/cl_pa_pb_dlv_shp_coll_bc=>new_for( im_likp = likp
                                                       im_lipss = lipss
                                                       im_vbpas = vbpas
                                                       im_vekps = vekps
                                                       im_vepos = vepos ).

shipment = collector_bc->create_shipment( org_unit ).
Write: 'Collected data: ' , shipment-referencenumber1.
WRITE /.

*---- Workstation ---
workstation_id = /aeb/cl_pa_pb_wsta_id_bc=>new( )->get_workstation_id( ).


*---- Build up the data structure for the request ------
CREATE OBJECT request.
request->set_client_identcode( engn_prm_mo->get_engine_client( ) ).
request->set_client_system_id( sy-sysid && '_' && sy-mandt ).  "system Id must be like P01_400
request->set_username( sy-uname ).

request->set_shipment( shipment ). "adds the collected data to the request

APPEND 'DE' TO result_language_iso_codes.
APPEND 'EN' TO result_language_iso_codes.
request->set_result_language_iso_codes( result_language_iso_codes ).

*--- Creation parameter for labels ---
CREATE OBJECT creation_parms.
creation_parms->set_creation_mode( 'ALWAYS' )."create labels in any case
request->set_creation_parms( creation_parms ).

*--- Processing parameters ---
CREATE OBJECT process_parms.
process_parms->set_do_completion( '-' )."set value to X if you want to close the shipment 
process_parms->set_workstation_id( workstation_id ).
process_mode-mode = 'EXTENDED'.
process_parms->set_process_mode( process_mode ).

CREATE OBJECT output_mode.
output_mode->set_mode( 'RETURN' ).
process_parms->set_document_output_mode( output_mode ).

CREATE OBJECT prepare_scope.
prepare_scope->set_scope( 'ALL' ).
process_parms->set_document_prepare_scope( prepare_scope ).

CREATE OBJECT output_scope.
output_scope->set_scope( 'ALL' ).
process_parms->set_document_output_scope( output_scope ).

request->set_process_parms(  process_parms ).


*---- send the request to Carrier Connect  ------
TRY.
    carrier_bf = /aeb/cl_pa_pb_carrier_bf=>new_for( im_engn_prm = engn_prm_mo
                                                    im_org_unit = org_unit ).

    result = carrier_bf->create_shipment( request ).
    IF result->get_has_errors( ) = 'X'.
      LOOP AT result->get_messages( ) INTO msg.
        LOOP AT msg->get_message_texts( ) INTO text.
          WRITE text->get_text( ).
          WRITE /.
        ENDLOOP.
      ENDLOOP.
    ELSE.
      WRITE result->get_shipment_number( ).
    ENDIF.
  CATCH /aeb/cx_01_pb_bf_inv_sc INTO exc.
    WRITE exc->/aeb/if_01_cx_message~get_msg_as_str( ).
ENDTRY.
```