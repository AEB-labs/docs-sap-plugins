---
title: Attach documents to a shipping order
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
You can attach documents to an already existing shipping order by using the method ADD_SHIPMENT_ATTACHMENTS of class /AEB/CL_PA_PB_CARRIER_BF.  

> 🚧 Attaching documents is only possible if the shipping order is not marked as completed.

Example coding:  

```text Attach documents
DATA: lt_attachements TYPE /aeb/cl_pa_pb_attachment_do=>tt_attachment_do,
      lo_attachement  TYPE REF TO /aeb/cl_pa_pb_attachment_do,
      lv_data         TYPE /aeb/01_rawstring,
      ls_torrot       TYPE  /scmtms/d_torrot,
      lo_request      TYPE REF TO /aeb/cl_pa_pb_add_satt_req_do,
      lt_langs        TYPE /aeb/01_char2s,
      lv_lang         TYPE /aeb/01_char2 VALUE 'DE',
      org_unit				TYPE char30.
     

data(lo_org_unit_bc) = /ae1/cl_pa_pb_fro_ou_rule_bc=>new_for( im_torrot = ls_torrot ).
data(org_unit) = lo_org_unit_bc->get_org_unit( ).

DATA(lo_connection_parms_bc) = /aeb/cl_pa_pb_engn_prm_bc=>get_instance( ).
DATA(lo_engn_prm) = lo_connection_parms_bc->get_engn_prm_do_for( org_unit ).

DATA(lo_carrier_bf) = /aeb/cl_pa_pb_carrier_bf=>new_for(
                        im_engn_prm = lo_engn_prm
                        im_org_unit = org_unit
                      ).
"Reference here is determined for freight order - use according ref_bc class for other objects
DATA(lo_reference_bc) = /ae1/cl_pa_pb_fro_shp_ref_bc=>new_for( im_torrot = ls_torrot ).
DATA(lv_shp_ref) = lo_reference_bc->get_shp_ref_do( ).

CREATE OBJECT lo_attachement.
*lv_data = "Needs to be provided with the proper binary content of the document
lo_attachement->set_data( im_value = lv_data ).
lo_attachement->set_file_name( im_value = 'ABD_' && ls_torrot-tor_id && '.pdf' ).
lo_attachement->set_content_type( im_value = 'EXPORT_ACCOMPANYING_DOCUMENT' ). "COMMERCIAL_INVOICE / PRO_FORMA_INVOICE
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
    "...
ENDTRY.
```



For more information about the handling of documents in Carrier Cloud, see here: [Attach documents](https://transport-freight-management.docs.developers.aeb.com/docs/transmitting-documents-to-carrier-connect)