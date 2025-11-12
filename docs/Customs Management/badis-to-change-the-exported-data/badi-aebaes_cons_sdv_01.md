---
title: BAdI /AEB/AES_CONS_SDV_01 (Delivery)
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: badi-aebaes_cons_inv_01
      title: BAdI /AEB/AES_CONS_INV_01
---
```text Change goods description of the first delivery
DATA:
  deliveries            TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,
  first_delivery        TYPE REF TO /aeb/if_aes_pb_delivery_do,
  tmp_goods_description TYPE REF TO /aeb/if_aes_pb_text_in_lan_do,
  goods_description     TYPE /aeb/if_aes_pb_text_in_lan_do=>tt_text_in_lan_do.

deliveries = im_value->get_deliveries( ).
READ TABLE deliveries INTO first_delivery INDEX 1.
IF sy-subrc <> 0.
* There are no deliveries in this consignment
  RETURN.
ENDIF.

tmp_goods_description = im_cons_data_object_factory->new_aes_pb_text_in_lan_do(
  im_language_iso_code = 'EN'
  im_text = 'Sample goods description' ).
APPEND tmp_goods_description TO goods_description.

tmp_goods_description = im_cons_data_object_factory->new_aes_pb_text_in_lan_do(
  im_language_iso_code = 'DE'
  im_text = 'Beispielwarenbeschreibung' ).
APPEND tmp_goods_description TO goods_description.

first_delivery->set_goods_description( goods_description ).
```
