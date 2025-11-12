---
title: Replace Invoice number with delivery number
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
In this example we determine the delivery number of a invoice with the help of the document flow and replace the invoice number in the delivery note with it.
[block:code]
{
  "codes": [
    {
      "code": "DATA:\n  ae41trans_do TYPE REF TO /aeb/if_ct_pb_ae41trans_do,\n  char_30_nv   TYPE REF TO /aeb/cl_01_char_30_nv,\n  comwa        TYPE vbco6,\n  t_vbfas      TYPE vbfa_t,\n  vbfa         TYPE vbfa,\n  trans_cde    TYPE c LENGTH 30.\n\nae41trans_do = im_as4trans->get_ae41trans( ).\nchar_30_nv = ae41trans_do->get_trans_cde( ).\ncomwa-mandt = sy-mandt.\ncomwa-vbeln = char_30_nv->v.\n\nCALL FUNCTION 'RV_ORDER_FLOW_INFORMATION'\n  EXPORTING\n    comwa      = comwa\n    nachfolger = '-'\n  TABLES\n    vbfa_tab   = t_vbfas.\n\nREAD TABLE t_vbfas INTO vbfa WITH KEY vbtyp_v = 'J'.\n\ntrans_cde = vbfa-vbelv.\nchar_30_nv = im_nullable_value_factory->char_30( trans_cde ).\nae41trans_do->set_trans_cde( char_30_nv ).",
      "language": "text",
      "name": "Replace AE41TRANS trans_cde"
    }
  ]
}
[/block]