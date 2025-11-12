---
title: Change determined data
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: badi-aebaes_cons_shp_01
      title: BAdI /AEB/AES_CONS_SHP_01 (Shipment)
---
These examples are not intended for a 1:1 copy, but to demonstrate the concepts and how to use certain patterns. Sometimes they use parameters which are only available in the context of the SAP business object like im\_likp , im\_vttk, etc.  Please adapt the examples to the context of the certain BAdI you are just implementing.    

## Change the remark on consignment level

```text Change a single field value
DATA:
    remark TYPE /aeb/if_aes_pb_consignment_do=>t_remark.

  CONCATENATE 'Invoice number: ' im_vbrk-vbeln ', invoice date: ' im_vbrk-fkdat
         INTO remark
         SEPARATED BY space.
  im_value->set_remark( remark ).
```

## Change the profile

```text Changing the profile based on shipping condition
IF im_vttk-vsbed = '01'.
*The profile code assigned here needs to be setup in Customs Management, 
*but not in SAP add-on 
    im_value->set_profile_code( 'SPECIAL_PROFILE' ).
  ENDIF.
```

## Change the net mass on delivery level

```text Set the total net mass of the first delivery to 500 KG
DATA:
    total_net_mass_nv TYPE REF TO /aeb/cl_01_dec_17_3_nv,
    total_net_mass    TYPE REF TO /aeb/if_aes_pb_quantity_do,
    deliveries        TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,
    first_delivery    TYPE REF TO /aeb/if_aes_pb_delivery_do.

  total_net_mass_nv = im_nullable_value_factory->dec_17_3( '500.00' ).
  total_net_mass = im_cons_data_object_factory->new_aes_pb_quantity_do_v1(
                        im_unit  = 'KG'
                        im_value = total_net_mass_nv ).

  deliveries = im_value->get_deliveries( ).
  READ TABLE deliveries INTO first_delivery INDEX 1.
  first_delivery->set_total_net_mass( total_net_mass ).
```

## Change the item quantity

```text Change the quantity of the first item in the first delivery to 1 piece
DATA:
    quantity_nv      TYPE REF TO /aeb/cl_01_dec_17_3_nv,
    new_quantity     TYPE REF TO /aeb/if_aes_pb_quantity_do,
    deliveries       TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,
    first_delivery   TYPE REF TO /aeb/if_aes_pb_delivery_do,
    items            TYPE /aeb/if_aes_pb_delivery_itm_do=>tt_delivery_item_do,
    first_item       TYPE REF TO /aeb/if_aes_pb_delivery_itm_do,
    quantities       TYPE /aeb/if_aes_pb_ic_quantity_do=>tt_ic_quantity_do,
    current_quantity TYPE REF TO /aeb/if_aes_pb_ic_quantity_do.

  quantity_nv = im_nullable_value_factory->dec_17_3( 1 ).
  new_quantity = im_cons_data_object_factory->new_aes_pb_quantity_do_v1( im_unit  = 'ST'
                                                                         im_value = quantity_nv ).

  deliveries = im_value->get_deliveries( ).
  READ TABLE deliveries INTO first_delivery INDEX 1.
  items = first_delivery->get_items( ).
  READ TABLE items INTO first_item INDEX 1.
  quantities = first_item->get_quantities( ).
  LOOP AT quantities INTO current_quantity.
    IF current_quantity->get_quantity_type( ) = 'ITEM'.
      current_quantity->set_quantity( new_quantity ).
    ENDIF.
  ENDLOOP.
```

## Set the EORI number of a party

```Text Set EORI number
    DATA: lv_partner     TYPE bu_partner,
          lt_customs_ids TYPE /aeb/if_aes_pb_cust_ident_do=>tt_cust_ident_do,
          lv_idnumber    TYPE /aeb/if_aes_pb_cust_ident_do=>t_identification
          .

    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_parties) = lo_delivery->get_parties( ).
      LOOP AT lt_parties INTO DATA(lo_party).
        IF lo_party->get_party_type( ) = 'CARRIER'.
          DATA(lv_company_number) = lo_party->get_company_number( ).
          CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
            EXPORTING
              input  = lv_company_number
            IMPORTING
              output = lv_partner.
          SELECT SINGLE idnumber FROM but0id WHERE partner = @lv_partner AND type = 'SLLEOR' INTO @lv_idnumber.
          IF sy-subrc = 0.
            DATA(lo_customs_id) = im_cons_data_object_factory->new_aes_pb_cust_ident_do(
                                    im_identification      = lv_idnumber
                                    im_identification_type = 'EORI'
                                  ).
            APPEND lo_customs_id TO lt_customs_ids.
            lo_party->set_customs_ids( im_value = lt_customs_ids ).
          ENDIF.
        ENDIF.
      ENDLOOP.
    ENDLOOP.

```

## Set the aggregation or splitting criteria for an item

For more information about the meaning of the aggregation and split criteria , see: [https://customsmanagement.docs.developers.aeb.com/docs/aggregation-and-split](https://customsmanagement.docs.developers.aeb.com/docs/aggregation-and-split) 

This example shows how to set the aggregation criteria, but one can also use it the same way for setting the split criteria by calling "set\_additional\_split\_criteria( 'this is the value of the split criteria' )".     

```text Set AdditionalAggregationKey
DATA:
  deliveries          TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,
  curr_delivery       TYPE REF TO /aeb/if_aes_pb_delivery_do,
  items               TYPE /aeb/if_aes_pb_delivery_itm_do=>tt_delivery_item_do,
  curr_item           TYPE REF TO /aeb/if_aes_pb_delivery_itm_do,
  add_aggregation_key TYPE REF TO /aeb/cl_01_char_100_nv.

deliveries = im_value->get_deliveries( ).

LOOP AT deliveries INTO curr_delivery.
  items = curr_delivery->get_items( ).
  LOOP AT items INTO curr_item.
    add_aggregation_key = im_nullable_value_factory->char_100( '3LLA' ).
    curr_item->set_additional_aggregation_key( add_aggregation_key ).
  ENDLOOP.
ENDLOOP.
```

## Set the security consignee in the delivery

```text Set security consignee
DATA:
deliveries         TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,
curr_delivery      TYPE REF TO /aeb/if_aes_pb_delivery_do,
secu_consignee     TYPE REF TO /aeb/if_aes_pb_party_do.   

deliveries = im_value->get_deliveries( ).

 LOOP AT deliveries INTO curr_delivery.
 secu_consignee = im_cons_data_object_factory->new_aes_pb_party_do( ).
        parties = curr_delivery->get_parties( ).
        LOOP AT parties INTO party.
          IF party->get_party_type( ) = 'CONSIGNEE'.
            fld_city          = party->get_city( ).
            fld_c_number      = party->get_company_number( ).
            fld_country       = party->get_country( ).
            fld_district      = party->get_district( ).
            fld_name          = party->get_name( ).
            fld_post_code     = party->get_post_code( ).
            fld_street        = party->get_street( ).
            secu_consignee->set_city( fld_city ).
            secu_consignee->set_company_number( fld_c_number ).
            secu_consignee->set_country( fld_country ).
            secu_consignee->set_district( fld_district ).
            secu_consignee->set_name( fld_name ).
            secu_consignee->set_post_code( fld_post_code ).
            secu_consignee->set_street( fld_street ).
            secu_consignee->set_party_type( 'CONSIGNEE_SECURITY' ).
            APPEND secu_consignee TO parties.
          ENDIF.
        ENDLOOP.
        curr_delivery->set_parties( parties ).
  ENDLOOP.##
```

## Change goods description of a delivery

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
