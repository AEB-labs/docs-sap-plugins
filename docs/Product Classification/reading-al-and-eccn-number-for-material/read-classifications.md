---
title: Read classification certificates
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
In this example we are looking up the certificate data of a specific material for a plant and a destination country. 
[block:code]
{
  "codes": [
    {
      "code": "DATA:\n  bc           TYPE REF TO /aeb/cl_ta_pb_smat_read_v1_bc,\n  certificates TYPE /aeb/if_ta_pb_sync_cert_do=>tt_sync_cert_do,\n  certificate  TYPE REF TO /aeb/if_ta_pb_sync_cert_do.\n\nbc  = /aeb/cl_ta_pb_smat_read_v1_bc=>new_for( im_material_no = 'M-11'\n                                              im_material_plant = '1010' ).\n\ncertificates = bc->get_certificates_for( im_decisive_date = sy-datum\n                                   im_dest_ctry_iso_code = 'US' ).\nLOOP AT certificates into certificate.\n  write certificate->get_additional_expl( ).\n  write certificate->get_cert_country( ).\n  write certificate->get_code( ).\n  write certificate->get_date_of_issue( ).\n  write certificate->get_detail( ).\n  write certificate->get_end_of_validity( ).\n  write certificate->get_geo_id( ).\n  write certificate->get_qualifier( ).\n  write certificate->get_reference( ).\n  write certificate->get_valid_from( ).\n  write certificate->get_valid_to( ).\nENDLOOP.",
      "language": "text",
      "name": "Get certificates for destination country"
    }
  ]
}
[/block]
The alternative is to get all certificates of the material and plant, like in the following example.
[block:code]
{
  "codes": [
    {
      "code": "DATA:\n  bc           TYPE REF TO /aeb/cl_ta_pb_smat_read_v1_bc,\n  certificates TYPE /aeb/if_ta_pb_sync_cert_do=>tt_sync_cert_do,\n  certificate  TYPE REF TO /aeb/if_ta_pb_sync_cert_do.\n\nbc  = /aeb/cl_ta_pb_smat_read_v1_bc=>new_for( im_material_no = 'M-11'\n                                              im_material_plant = '1010' ).\n\ncertificates = bc->get_certificates( ).\nLOOP AT certificates into certificate.\n  write certificate->get_additional_expl( ).\n  write certificate->get_cert_country( ).\n  write certificate->get_code( ).\n  write certificate->get_date_of_issue( ).\n  write certificate->get_detail( ).\n  write certificate->get_end_of_validity( ).\n  write certificate->get_geo_id( ).\n  write certificate->get_qualifier( ).\n  write certificate->get_reference( ).\n  write certificate->get_valid_from( ).\n  write certificate->get_valid_to( ).\nENDLOOP.",
      "language": "text",
      "name": "Get all certificates"
    }
  ]
}
[/block]