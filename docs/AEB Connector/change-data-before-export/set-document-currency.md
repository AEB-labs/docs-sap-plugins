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
[block:code]
{
  "codes": [
    {
      "code": "DATA:\n  waerk           TYPE c LENGTH 3,\n  curr_iso        TYPE REF TO /aeb/cl_01_char_3_nv,\n  ae41cons_do     TYPE REF TO /aeb/if_ct_pb_ae41cons_do,\n  tt_ae41trans_do TYPE /aeb/if_ct_pb_ae41trans_do=>tt_ae41trans_do,\n  ae41trans_do    TYPE REF TO /aeb/if_ct_pb_ae41trans_do.\n\nwaerk = im_likp-waerk. \"or im_vbrk for invoices\ncurr_iso = im_nullable_value_factory->char_3( waerk ).\nae41cons_do = im_as4cons->get_ae41cons( ).\ntt_ae41trans_do = ae41cons_do->get_ae41trans( ).\n\nLOOP AT tt_ae41trans_do INTO ae41trans_do .\n  ae41trans_do->set_curr_iso( curr_iso ).\nENDLOOP.",
      "language": "text",
      "name": "Export consignment"
    },
    {
      "code": "DATA:\n  waerk           TYPE c LENGTH 3,\n  curr_iso        TYPE REF TO /aeb/cl_01_char_3_nv,\n  ae41trans_do    TYPE REF TO /aeb/if_ct_pb_ae41trans_do.\n\nwaerk = im_likp-waerk. \"or im_vbrk for invoices\ncurr_iso = im_nullable_value_factory->char_3( waerk ).\nae41trans_do = im_as4cons->get_ae41trans( ).\nae41trans_do->set_curr_iso( curr_iso ).",
      "language": "text",
      "name": "Delivery note"
    }
  ]
}
[/block]