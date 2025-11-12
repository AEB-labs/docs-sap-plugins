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
[block:code]
{
  "codes": [
    {
      "code": "DATA:\n  deliveries            TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,\n  first_delivery        TYPE REF TO /aeb/if_aes_pb_delivery_do,\n  tmp_goods_description TYPE REF TO /aeb/if_aes_pb_text_in_lan_do,\n  goods_description     TYPE /aeb/if_aes_pb_text_in_lan_do=>tt_text_in_lan_do.\n\ndeliveries = im_value->get_deliveries( ).\nREAD TABLE deliveries INTO first_delivery INDEX 1.\nIF sy-subrc <> 0.\n* There are no deliveries in this consignment\n  RETURN.\nENDIF.\n\ntmp_goods_description = im_cons_data_object_factory->new_aes_pb_text_in_lan_do(\n  im_language_iso_code = 'EN'\n  im_text = 'Sample goods description' ).\nAPPEND tmp_goods_description TO goods_description.\n\ntmp_goods_description = im_cons_data_object_factory->new_aes_pb_text_in_lan_do(\n  im_language_iso_code = 'DE'\n  im_text = 'Beispielwarenbeschreibung' ).\nAPPEND tmp_goods_description TO goods_description.\n\nfirst_delivery->set_goods_description( goods_description ).",
      "language": "text",
      "name": "Change goods description of the first delivery"
    }
  ]
}
[/block]