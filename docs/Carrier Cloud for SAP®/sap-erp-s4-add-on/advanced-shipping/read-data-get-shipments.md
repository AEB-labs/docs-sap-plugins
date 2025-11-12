---
title: Read data of a shipping order (get shipments)
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
The communication from SAP to Carrier Cloud is synchronous, so you'll receive data in the response of the call ( result parameter), which you can use for further processing. But you can also trigger another call to Carrier Cloud to read the data of a shipping order, using method GET\_SHIPMENTS of class /AEB/CL\_PA\_PB\_CARRIER\_BF.

This program first determines the values required for the call (org unit, connection parameters, reference) and then gets the data from the according shipping orders: 

```text Get shipment
REPORT zaeb_get_first_shipment.

PARAMETERS: vbeln TYPE likp-vbeln.

DATA:
  likp             TYPE likp,
  lipss            TYPE STANDARD TABLE OF lips,
  org_unit_rule_bc TYPE REF TO /aeb/cl_pa_pb_dlv_ou_rule_bc,
  org_unit         TYPE /aeb/01_char20,
  engn_prm_bc      TYPE REF TO /aeb/cl_pa_pb_engn_prm_bc,
  engn_prm_mo      TYPE REF TO /aeb/if_pa_pb_engn_prm_mo,
  shp_ref_bc       TYPE REF TO /aeb/cl_pa_pb_dlv_shp_ref_bc,
  shipment_ref     TYPE /aeb/pa_pb_dl_shp_ref_do,
  carrier_bf       TYPE REF TO /aeb/cl_pa_pb_carrier_bf.

SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.
SELECT * FROM lips INTO TABLE lipss WHERE vbeln = vbeln.

org_unit_rule_bc = /aeb/cl_pa_pb_dlv_ou_rule_bc=>new_for( im_likp = likp im_lipss = lipss ).
org_unit = org_unit_rule_bc->get_org_unit( ).

engn_prm_bc = /aeb/cl_pa_pb_engn_prm_bc=>get_instance( ).
engn_prm_mo = engn_prm_bc->get_engn_prm_do_for( org_unit ).

shp_ref_bc = /aeb/cl_pa_pb_dlv_shp_ref_bc=>new_for( likp ).
shipment_ref = shp_ref_bc->get_shp_ref_do( ).

DATA:
  request                   TYPE REF TO /aeb/cl_pa_pb_get_shp_req_do,
  result_language_iso_codes TYPE /aeb/01_char2s,
  result                    TYPE REF TO /aeb/cl_pa_pb_get_shp_res_do,
  exc                       TYPE REF TO /aeb/cx_01_pb_bf_inv_sc,
  msg                       TYPE REF TO /aeb/cl_01_pb_res_msg_do,
  text                      TYPE REF TO /aeb/cl_01_pb_til_do,
  shp_refs                  TYPE /aeb/pa_pb_dl_shp_ref_dos,
  shipment                  TYPE /aeb/pa_pb_dl_shp_res_do.

CREATE OBJECT request.
request->set_client_identcode( engn_prm_mo->get_engine_client( ) ).
request->set_client_system_id( sy-sysid && '_' && sy-mandt ).
request->set_username( sy-uname ).
APPEND 'DE' TO result_language_iso_codes.
APPEND 'EN' TO result_language_iso_codes.
request->set_result_language_iso_codes( result_language_iso_codes ).
APPEND shipment_ref TO shp_refs.
request->set_shipment_references( shp_refs ).

TRY.
    carrier_bf = /aeb/cl_pa_pb_carrier_bf=>new_for( im_engn_prm = engn_prm_mo
                                                    im_org_unit = org_unit ).

    result = carrier_bf->get_shipments( request ).
    IF result->get_has_errors( ) = 'X'.
      LOOP AT result->get_messages( ) INTO msg.
        LOOP AT msg->get_message_texts( ) INTO text.
          WRITE text->get_text( ).
          WRITE /.
        ENDLOOP.
      ENDLOOP.
    ELSE.
      loop at result->get_shipments( ) into shipment.
        write shipment-referencenumber1.
      endloop.
    ENDIF.
  CATCH /aeb/cx_01_pb_bf_inv_sc INTO exc.
    WRITE exc->/aeb/if_01_cx_message~get_msg_as_str( ).
ENDTRY.
```
