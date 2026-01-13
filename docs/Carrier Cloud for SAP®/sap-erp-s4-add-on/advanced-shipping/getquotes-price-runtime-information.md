---
title: 'GetQuotes for pricing and runtime information '
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
To request pricing and runtime information for a certain carrier, use method _get_quotes_  of the class /AEB/CL_PA_PB_BSMCARRIER_BF.

You can call this service even if the shipment has not been created yet in Carrier Connect.

```Text Get quotes
DATA:
  likp                   TYPE likp,
  lipss                  TYPE STANDARD TABLE OF lips,
  vbpas                  TYPE STANDARD TABLE OF vbpa,
  vekps                  TYPE STANDARD TABLE OF vekp,
  vepos                  TYPE STANDARD TABLE OF vepo,
  bsmcarrier_bf          TYPE REF TO /aeb/cl_pa_pb_bsmcarrier_bf,
  shipment_coll          TYPE REF TO /aeb/cl_pa_pb_dlv_shp_coll_bc,
  get_quot_req_do        TYPE REF TO /aeb/cl_pa_pb_get_quot_req_do,
  get_quot_res_do        TYPE REF TO /aeb/cl_pa_pb_get_quot_res_do,
  shipment               TYPE /aeb/pa_pb_dl_shp_req_do,
  engn_prm_bc            TYPE REF TO /aeb/cl_pa_pb_engn_prm_bc,
  engn_prm_mo            TYPE REF TO /aeb/if_pa_pb_engn_prm_mo,
  org_unit_rule_bc       TYPE REF TO /aeb/cl_pa_pb_dlv_ou_rule_bc,
  org_unit               TYPE /aeb/01_char20,
  exception              TYPE REF TO /aeb/cx_01_pb_bf_inv_sc,
  result_langu_iso_codes TYPE /aeb/01_char2s,
  msg                    TYPE REF TO /aeb/cl_01_pb_res_msg_do,
  text                   TYPE REF TO /aeb/cl_01_pb_til_do,
  vbeln                  TYPE vbeln,
  quoted_services        TYPE /aeb/cl_pa_pb_get_quot_serv_do=>tt_pb_get_quot_serv_do.

SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.
SELECT * FROM lips INTO TABLE lipss WHERE vbeln = vbeln.
SELECT * FROM vbpa INTO TABLE vbpas WHERE vbeln = vbeln.
"Determine vekps for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB
"Determine vepos for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB

org_unit_rule_bc = /aeb/cl_pa_pb_dlv_ou_rule_bc=>new_for( im_likp = likp
                                                          im_lipss = lipss ).
org_unit = org_unit_rule_bc->get_org_unit( ).

engn_prm_bc = /aeb/cl_pa_pb_engn_prm_bc=>get_instance( ).
engn_prm_mo = engn_prm_bc->get_engn_prm_do_for( org_unit ).

bsmcarrier_bf = /aeb/cl_pa_pb_bsmcarrier_bf=>new_for( im_engn_prm = engn_prm_mo
                                                      im_org_unit = org_unit ).

shipment_coll = /aeb/cl_pa_pb_dlv_shp_coll_bc=>new_for( im_likp = likp
                                                        im_lipss = lipss
                                                        im_vbpas = vbpas
                                                        im_vekps = vekps
                                                        im_vepos = vepos ).
shipment = shipment_coll->create_shipment( org_unit ).

CREATE OBJECT get_quot_req_do.
APPEND 'DE' TO result_langu_iso_codes.
get_quot_req_do->set_result_language_iso_codes( result_langu_iso_codes ).
get_quot_req_do->set_client_identcode( engn_prm_mo->get_engine_client( ) ).
get_quot_req_do->set_client_system_id( sy-sysid && '_' && sy-mandt ).
get_quot_req_do->set_username( sy-uname ).

get_quot_req_do->set_shipment( shipment ).

TRY.
    get_quot_res_do = bsmcarrier_bf->get_quotes( get_quot_req_do ).
    IF get_quot_res_do->get_has_errors( ) = 'X'.
      LOOP AT get_quot_res_do->get_messages( ) INTO msg.
        LOOP AT msg->get_message_texts( ) INTO text.
          WRITE text->get_text( ).
          WRITE /.
        ENDLOOP.
      ENDLOOP.
    ELSE.
      quoted_services = get_quot_res_do->get_quoted_services( ).
      LOOP AT quoted_services INTO DATA(quoted_service).
        DATA(quoted_price) = quoted_service->get_data( )->get_net_price( ).
        DATA(transit_time) = quoted_service->get_data( )->get_transit_time( ).
        DATA(transit_date) = quoted_service->get_data( )->get_transit_date( ).
      ENDLOOP.
    ENDIF.
  CATCH /aeb/cx_01_pb_bf_inv_sc INTO exception.
    WRITE exception->/aeb/if_01_cx_message~get_msg_as_str( ).
ENDTRY.
```
