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

```text Get certificates for destination country
DATA:
  bc           TYPE REF TO /aeb/cl_ta_pb_smat_read_v1_bc,
  certificates TYPE /aeb/if_ta_pb_sync_cert_do=>tt_sync_cert_do,
  certificate  TYPE REF TO /aeb/if_ta_pb_sync_cert_do.

bc  = /aeb/cl_ta_pb_smat_read_v1_bc=>new_for( im_material_no = 'M-11'
                                              im_material_plant = '1010' ).

certificates = bc->get_certificates_for( im_decisive_date = sy-datum
                                   im_dest_ctry_iso_code = 'US' ).
LOOP AT certificates into certificate.
  write certificate->get_additional_expl( ).
  write certificate->get_cert_country( ).
  write certificate->get_code( ).
  write certificate->get_date_of_issue( ).
  write certificate->get_detail( ).
  write certificate->get_end_of_validity( ).
  write certificate->get_geo_id( ).
  write certificate->get_qualifier( ).
  write certificate->get_reference( ).
  write certificate->get_valid_from( ).
  write certificate->get_valid_to( ).
ENDLOOP.
```

The alternative is to get all certificates of the material and plant, like in the following example.

```text Get all certificates
DATA:
  bc           TYPE REF TO /aeb/cl_ta_pb_smat_read_v1_bc,
  certificates TYPE /aeb/if_ta_pb_sync_cert_do=>tt_sync_cert_do,
  certificate  TYPE REF TO /aeb/if_ta_pb_sync_cert_do.

bc  = /aeb/cl_ta_pb_smat_read_v1_bc=>new_for( im_material_no = 'M-11'
                                              im_material_plant = '1010' ).

certificates = bc->get_certificates( ).
LOOP AT certificates into certificate.
  write certificate->get_additional_expl( ).
  write certificate->get_cert_country( ).
  write certificate->get_code( ).
  write certificate->get_date_of_issue( ).
  write certificate->get_detail( ).
  write certificate->get_end_of_validity( ).
  write certificate->get_geo_id( ).
  write certificate->get_qualifier( ).
  write certificate->get_reference( ).
  write certificate->get_valid_from( ).
  write certificate->get_valid_to( ).
ENDLOOP.
```
