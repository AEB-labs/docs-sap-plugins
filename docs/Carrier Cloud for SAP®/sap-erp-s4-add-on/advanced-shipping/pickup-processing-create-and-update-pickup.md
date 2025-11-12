---
title: Pickup processing (create and update pickup)
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
To read more about the functionality of a pickup and how to use it in Carrier Cloud, see here: [Pickup processing](https://transport-freight-management.docs.developers.aeb.com/docs/pickup) 

In SAP, the add from AEB provides these methods for the handling of pickups: 

* CREATE\_PICKUP
* PROCESS\_PICKUP
* GET\_PICKUP and GET\_PICKUPS
* DELETE\_PICKUP

The following code snippet outlines the creation of a pickup. Precondition here is that the shipping orders that should be assigned to the pickup are created based on deliveries. Therefore it collects the references of those shipping orders using the class /AEB/CL\_PA\_PB\_DLV\_SHP\_REF\_BC. If your process is based on other business objects from SAP, use the according class.   

```text Create pickup
"Create pickup call request
DATA: lo_request         TYPE REF TO /aeb/cl_pa_pb_cr_pu_req_do,
      ls_creation_parms  TYPE /aeb/pa_pb_dl_pcp_do,
      ls_process_parms   TYPE /aeb/pa_pb_dl_ppp_do,
      lt_langs           TYPE /aeb/01_char2s,
      ls_pickup          TYPE /aeb/pa_pb_dl_prd_do,
      lt_references	     TYPE /aeb/pa_pb_dl_shp_ref_dos,
      lv_carrier         TYPE /aeb/01_char20,
      lv_vstel           TYPE vstel,
      lo_carrier_bf      TYPE REF TO /aeb/cl_pa_pb_carrier_bf,
      lo_engn_prm        TYPE REF TO /aeb/if_pa_pb_engn_prm_mo,
      lo_engn_prm_bc     TYPE REF TO /aeb/cl_pa_pb_engn_prm_bc,
      lv_org_unit	       TYPE /aeb/01_char20
      .

"Default values - need to be changed to your own logic
lv_org_unit = 'XYZ'. "Please enter correct org unit here or use AEB determination e.g. /AEB/CL_PA_PB_DLV_OU_RULE_BC
lv_vstel = '9999'. "Please enter correct data here
ls_pickup-carrieridentcode = 'XYZ'. "Please use correct carrier ident code here

"Determine list of LIKPs first
SELECT * FROM likp WHERE wadat = @sy-datum AND vstel = @lv_vstel INTO TABLE @DATA(lt_likps). "Change to your own logic!
LOOP AT lt_likps INTO DATA(ls_likp).
  DATA(lo_dlv_shp_ref) = /aeb/cl_pa_pb_dlv_shp_ref_bc=>new_for( im_likp = ls_likp ).
  DATA(ls_shp_ref) = lo_dlv_shp_ref->get_shp_ref_do( ).
  APPEND ls_shp_ref TO lt_references.
ENDLOOP.

IF lt_references IS INITIAL.
  RETURN.
ENDIF.

CREATE OBJECT lo_request.
lo_request->set_client_system_id( im_value = sy-sysid && '_' && sy-mandt ).
ls_creation_parms-creationmode = 'ONLY_VALID_SHIPMENTS'.
lo_request->set_creation_parms( im_value = ls_creation_parms ).
ls_process_parms-documentoutputmode-mode = 'RETURN'. "Might by different if you use the AEB print agent!
ls_process_parms-domanifest = 'X'. "Depends on your business process. This will complete your pickup.
ls_process_parms-workstationid = /aeb/cl_pa_pb_wsta_id_bc=>new( )->get_workstation_id( ).
lo_request->set_process_parms( im_value = ls_process_parms ).

APPEND 'de' TO lt_langs.
lo_request->set_result_language_iso_codes( im_value = lt_langs ).

lo_request->set_username( im_value = sy-uname ).

ls_pickup-shipments = lt_references.
ls_pickup-shippingdate = sy-datum. "Must match the shipping date of the shipping orders in Carrier Connect
ls_pickup-shippingpt-companynumber = lv_vstel.
lo_request->set_pickup( im_value = ls_pickup ).

lo_engn_prm_bc = /aeb/cl_pa_pb_engn_prm_bc=>get_instance( ).
lo_engn_prm = lo_engn_prm_bc->get_engn_prm_do_for( im_org_unit = lv_org_unit ).

lo_carrier_bf = /aeb/cl_pa_pb_carrier_bf=>new_for(
                  im_engn_prm = lo_engn_prm
                  im_org_unit = lv_org_unit
                ).

lo_request->set_client_identcode( im_value = lo_engn_prm->get_engine_client( ) ).

TRY.
    DATA(lo_pickup) = lo_carrier_bf->create_pickup( im_request = lo_request ).
  CATCH /aeb/cx_01_pb_bf_inv_sc. " Interface exception
    "Implement suitable error handling here
    RETURN.
ENDTRY.

IF lo_pickup->get_has_errors( ) = 'X'.
  "Create error message and log entry here
ELSEIF lo_pickup->get_has_warnings( ) = 'X'.
  "Create warning message and log entry
ELSE.
  "Print manifest
ENDIF.
```
