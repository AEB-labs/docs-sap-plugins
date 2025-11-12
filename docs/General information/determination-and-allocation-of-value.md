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

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Example
      </th>

      <th style={{ textAlign: "left" }}>
        Meaning
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Document\_value\_as\_nv =
        im\_nullable\_value\_factory->dec\_15\_2( 0 ).
      </td>

      <td style={{ textAlign: "left" }}>
        Value is 0.00.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Document\_value\_as\_nv =\
        im\_nullable\_value\_factory->dec\_15\_2( ‘15.23’ ).
      </td>

      <td style={{ textAlign: "left" }}>
        Value is 15.23.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Clear document\_value\_as\_nv.
      </td>

      <td style={{ textAlign: "left" }}>
        instance is "zero"/null and the value is not interpreted.
      </td>
    </tr>
  </tbody>
</Table>

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
