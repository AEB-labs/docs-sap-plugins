---
title: GetShipment for price determination
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
This service determines the pricing (charges) for an existing shipment in Carrier Connect.  Use the method *get_shipment* of the class /AEB/CL_PA_PB_BSMCARRIER_BF.

```
DATA:
  likp                   TYPE likp,
  lipss                  TYPE STANDARD TABLE OF lips,
  bsmcarrier_bf          TYPE REF TO /aeb/cl_pa_pb_bsmcarrier_bf,
  ref_bc                 TYPE REF TO /aeb/cl_pa_pb_dlv_shp_ref_bc,
  get_ship_req_do        TYPE REF TO /aeb/cl_pa_pb_get_ship_req_do,
  get_ship_res_do        TYPE REF TO /aeb/cl_pa_pb_get_ship_res_do,
  engn_prm_bc            TYPE REF TO /aeb/cl_pa_pb_engn_prm_bc,
  engn_prm_mo            TYPE REF TO /aeb/if_pa_pb_engn_prm_mo,
  org_unit_rule_bc       TYPE REF TO /aeb/cl_pa_pb_dlv_ou_rule_bc,
  org_unit               TYPE /aeb/01_char20,
  exception              TYPE REF TO /aeb/cx_01_pb_bf_inv_sc,
  result_langu_iso_codes TYPE /aeb/01_char2s,
  msg                    TYPE REF TO /aeb/cl_01_pb_res_msg_do,
  text                   TYPE REF TO /aeb/cl_01_pb_til_do,
  reference              TYPE /aeb/pa_pb_dl_shp_ref_do,
  vbeln                  TYPE vbeln,
  quote_data 						 TYPE REF TO /AEB/CL_PA_PB_GET_QUOT_DATA_DO.

SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.
SELECT * FROM lips INTO TABLE lipss WHERE vbeln = vbeln.

org_unit_rule_bc = /aeb/cl_pa_pb_dlv_ou_rule_bc=>new_for( im_likp = likp
                                                          im_lipss = lipss ).
org_unit = org_unit_rule_bc->get_org_unit( ).

engn_prm_bc = /aeb/cl_pa_pb_engn_prm_bc=>get_instance( ).
engn_prm_mo = engn_prm_bc->get_engn_prm_do_for( org_unit ).

bsmcarrier_bf = /aeb/cl_pa_pb_bsmcarrier_bf=>new_for( im_engn_prm = engn_prm_mo
                                                      im_org_unit = org_unit ).

ref_bc = /aeb/cl_pa_pb_dlv_shp_ref_bc=>new_for( im_likp = likp ).
reference = ref_bc->get_shp_ref_do( ).

CREATE OBJECT get_ship_req_do.
APPEND 'DE' TO result_langu_iso_codes.
get_ship_req_do->set_result_language_iso_codes( result_langu_iso_codes ).
get_ship_req_do->set_client_identcode( engn_prm_mo->get_engine_client( ) ).
get_ship_req_do->set_client_system_id( sy-sysid && '_' && sy-mandt ).
get_ship_req_do->set_username( sy-uname ).

get_ship_req_do->set_include_documents( '-' ).
get_ship_req_do->set_reference( reference ).

TRY.
  get_ship_res_do = bsmcarrier_bf->get_shipment( get_ship_req_do ).
  quote_data = get_ship_res_do->get_shipment( )->get_quoted_data( ).
    IF get_ship_res_do->get_has_errors( ) = 'X'.
      LOOP AT get_ship_res_do->get_messages( ) INTO msg.
        LOOP AT msg->get_message_texts( ) INTO text.
          WRITE text->get_text( ).
          WRITE /.
        ENDLOOP.
      ENDLOOP.
    ENDIF.
  CATCH /aeb/cx_01_pb_bf_inv_sc INTO exception.
    WRITE exception->/aeb/if_01_cx_message~get_msg_as_str( ).
ENDTRY.
```
