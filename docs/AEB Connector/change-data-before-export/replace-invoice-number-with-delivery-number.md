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

```text Replace AE41TRANS trans_cde
DATA:
  ae41trans_do TYPE REF TO /aeb/if_ct_pb_ae41trans_do,
  char_30_nv   TYPE REF TO /aeb/cl_01_char_30_nv,
  comwa        TYPE vbco6,
  t_vbfas      TYPE vbfa_t,
  vbfa         TYPE vbfa,
  trans_cde    TYPE c LENGTH 30.

ae41trans_do = im_as4trans->get_ae41trans( ).
char_30_nv = ae41trans_do->get_trans_cde( ).
comwa-mandt = sy-mandt.
comwa-vbeln = char_30_nv->v.

CALL FUNCTION 'RV_ORDER_FLOW_INFORMATION'
  EXPORTING
    comwa      = comwa
    nachfolger = '-'
  TABLES
    vbfa_tab   = t_vbfas.

READ TABLE t_vbfas INTO vbfa WITH KEY vbtyp_v = 'J'.

trans_cde = vbfa-vbelv.
char_30_nv = im_nullable_value_factory->char_30( trans_cde ).
ae41trans_do->set_trans_cde( char_30_nv ).
```
