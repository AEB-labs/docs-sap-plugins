---
title: Set document currency
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The document currency is currently not set by default, but it can be easily set in the BAdI for deliveries and invoices.

```text Export consignment
DATA:
  waerk           TYPE c LENGTH 3,
  curr_iso        TYPE REF TO /aeb/cl_01_char_3_nv,
  ae41cons_do     TYPE REF TO /aeb/if_ct_pb_ae41cons_do,
  tt_ae41trans_do TYPE /aeb/if_ct_pb_ae41trans_do=>tt_ae41trans_do,
  ae41trans_do    TYPE REF TO /aeb/if_ct_pb_ae41trans_do.

waerk = im_likp-waerk. "or im_vbrk for invoices
curr_iso = im_nullable_value_factory->char_3( waerk ).
ae41cons_do = im_as4cons->get_ae41cons( ).
tt_ae41trans_do = ae41cons_do->get_ae41trans( ).

LOOP AT tt_ae41trans_do INTO ae41trans_do .
  ae41trans_do->set_curr_iso( curr_iso ).
ENDLOOP.
```
```text Delivery note
DATA:
  waerk           TYPE c LENGTH 3,
  curr_iso        TYPE REF TO /aeb/cl_01_char_3_nv,
  ae41trans_do    TYPE REF TO /aeb/if_ct_pb_ae41trans_do.

waerk = im_likp-waerk. "or im_vbrk for invoices
curr_iso = im_nullable_value_factory->char_3( waerk ).
ae41trans_do = im_as4cons->get_ae41trans( ).
ae41trans_do->set_curr_iso( curr_iso ).
```
