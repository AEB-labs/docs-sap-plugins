---
title: Determination and allocation of values
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
Even simple fields are represented as objects, so-called Nullable Values (recognizable by the ending \_NV). This makes it possible in the BAdI to differentiate whether a value is to be "zero" or empty. There is also a data object factory for Nullable Values, which is available as an importing parameter in the corresponding BAdIs.

[block:parameters]
{
  "data": {
    "h-0": "Example",
    "h-1": "Meaning",
    "0-0": "Document_value_as_nv =  \nim_nullable_value_factory->dec_15_2( 0 ).",
    "0-1": "Value is 0.00.",
    "1-0": "Document_value_as_nv =  \nim_nullable_value_factory->dec_15_2( ‘15.23’ ).",
    "1-1": "Value is 15.23.",
    "2-0": "Clear document_value_as_nv.",
    "2-1": "instance is \"zero\"/null and the value is not interpreted."
  },
  "cols": 2,
  "rows": 3,
  "align": [
    "left",
    "left"
  ]
}
[/block]


Character:

```text Example in BAdI /AEB/CMP_EC_ORDER_06
DATA:
    items     TYPE /aeb/if_cmp_pb_ec_item_do=>tt_ec_item_do,
    curr_item TYPE REF TO /aeb/if_cmp_pb_ec_item_do,
    prd       TYPE REF TO /aeb/if_cmp_pb_ec_prd_cls_do.

  items = im_value->get_items( ).
  LOOP AT items INTO curr_item.
    IF curr_item->get_material_number( )->v = 'MAT_TEST'.
      prd = im_data_object_factory->new_cmp_pb_ec_prd_cls_do( ).
      prd->set_classification_identcode( im_nullable_value_factory->char_50( 'ClassificationAusfuhrliste') ).
      prd->set_classification_number( im_nullable_value_factory->char_50( '0001' ) ).
      curr_item->add_product_classification( prd ).
    ENDIF.
  ENDLOOP.
```

Amount in currency:

```text Example in BAdI /AEB/AES_CONS_INV_01
DATA:
    new_aom  TYPE REF TO /aeb/if_aes_pb_aom_do,
    value_nv TYPE REF TO /aeb/cl_01_dec_17_2_nv.

  value_nv = im_nullable_value_factory->dec_17_2( '100.25' ).
  new_aom = im_cons_data_object_factory->new_aes_pb_aom_do_v1(
              im_currency_iso = 'EUR'
              im_value        = value_nv ).
```

Boolean:

```text Example in BAdI /AEB/PA_SHP_DLV_02
ch_shp_req_do-initpayerofchargaccnofromcompa = im_nullable_value_factory->boolean( im_value = 'X' ).      “ X means true, - means false 
```