---
title: 'Further common changes '
deprecated: false
hidden: false
metadata:
  robots: index
---
## General information
Following examples refer to the invoice badi /AEB/AES_CONS_INV_01. Most of the code will also work the same for other objects. Access to the order flow will of course have to be adjusted if other objects are used.

## Read commodity from material master (ECC) to update commodity export
Get list of classifications from item, delete existing value for commodity export, create new value, attach it to the list and set the extended list to the item.
```
DATA: lv_matnr TYPE matnr,
          lv_posnr TYPE posnr
          .

    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_items) = lo_delivery->get_items( ).
      LOOP AT lt_items INTO DATA(lo_item).
        DATA(lt_cls) = lo_item->get_classifications( ).
        LOOP AT lt_cls INTO DATA(lo_cls).
          IF lo_cls->get_type( ) = 'COCOEXPORT'.
            DELETE lt_cls. "Delete existing value
            EXIT.
          ENDIF.
        ENDLOOP.

        lv_posnr = lo_item->get_item_no( ).
        CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
          EXPORTING
            input  = lv_posnr
          IMPORTING
            output = lv_posnr.

        lv_matnr = lo_item->get_material_number( ).
        CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
          EXPORTING
            input        = lv_matnr
          IMPORTING
            output       = lv_matnr
          EXCEPTIONS
            length_error = 1
            OTHERS       = 2.
        IF sy-subrc <> 0.
* Implement suitable error handling here
        ENDIF.
        
        "Read data from material master
        SELECT SINGLE werks FROM vbrp WHERE vbeln = @im_vbrk-vbeln AND posnr = @lv_posnr INTO @DATA(lv_werks).
        SELECT SINGLE stawn FROM marc WHERE werks = @lv_werks AND matnr = @lv_matnr INTO @DATA(lv_stawn).
        REPLACE ALL OCCURRENCES OF '.' IN lv_stawn WITH ''.
        
        "Create new value
        lo_cls = im_cons_data_object_factory->new_aes_pb_cls_do(
                   im_type  = 'COCOEXPORT'
                   im_value = CONV #( lv_stawn )
                 ).
        APPEND lo_cls TO lt_cls.
        "Set new list
        lo_item->set_classifications( im_value = lt_cls ).
      ENDLOOP.
    ENDLOOP.
```

## Create packages from delivery header
```
DATA: ls_comwa         TYPE vbco6,
          lt_vbfas         TYPE TABLE OF vbfa,
          lt_cus_spec_refs TYPE /aeb/if_aes_pb_cus_sp_ref_do=>tt_aes_pb_cus_sp_ref_do
          .

    CALL FUNCTION 'RV_ORDER_FLOW_INFORMATION'
      EXPORTING
        comwa         = ls_comwa
      TABLES
        vbfa_tab      = lt_vbfas
      EXCEPTIONS
        no_vbfa       = 1
        no_vbuk_found = 2
        OTHERS        = 3.
    SORT lt_vbfas BY erdat DESCENDING erzet DESCENDING.
    LOOP AT lt_vbfas ASSIGNING FIELD-SYMBOL(<ls_vbfa>).
      IF <ls_vbfa>-vbtyp_n = 'J'.
        DATA(lv_vbeln_vl) = <ls_vbfa>-vbeln.
        EXIT.
      ENDIF.
    ENDLOOP.

    SELECT SINGLE * FROM likp WHERE vbeln = @lv_vbeln_vl INTO @DATA(ls_likp).

    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_packages) = lo_delivery->get_packages( ).
      DATA(lo_package) = im_cons_data_object_factory->new_aes_pb_package_do( ).
      APPEND lo_package TO lt_packages.
      DATA(lo_gross_mass_nv) = im_nullable_value_factory->dec_17_3( im_value = CONV #( ls_likp-btgew ) ).
      DATA(lo_gross_mass) = im_cons_data_object_factory->new_aes_pb_quantity_do_v1(
                              im_unit  = CONV #( ls_likp-gewei )
                              im_value = lo_gross_mass_nv
                            ).
      lo_package->set_gross_mass( im_value = lo_gross_mass ).
      lo_package->set_type_unece_code( im_value = 'PK' ).
      lo_package->set_quantity( im_value = CONV #( ls_likp-anzpk ) ).
      lo_package->set_marks( im_value = sy-sysid && '_' && sy-mandt && '_' && ls_likp-vbeln ).
      lo_package->set_package_id_client_system( im_value = sy-sysid && '_' && sy-mandt && '_' && ls_likp-vbeln ).
    ENDLOOP.
```

## Change person in charge
```
    DATA(lo_person) = im_cons_data_object_factory->new_aes_pb_person_do( ).
    lo_person->set_surname( im_value = 'Name' ).
    lo_person->set_phone( im_value = '1234' ).
    im_value->set_person_in_charge( im_value = lo_person ).
```

## Delete items with zero quantity
```
    DATA: lv_zero_quantitiy TYPE /aeb/01_boolean
          .

    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_items) = lo_delivery->get_items( ).
      LOOP AT lt_items INTO DATA(lo_item).
        lv_zero_quantitiy = '-'.
        DATA(lt_quantities) = lo_item->get_quantities( ).
        LOOP AT lt_quantities INTO DATA(lo_quantity).
          IF lo_quantity->get_quantity_type( ) = 'ITEM'.
            IF lo_quantity->get_quantity( ) IS INITIAL OR lo_quantity->get_quantity( )->get_value_v1( ) IS INITIAL OR lo_quantity->get_quantity( )->get_value_v1( )->v IS INITIAL.
              DELETE lt_items.
              lv_zero_quantitiy = 'X'.
              EXIT.
            ENDIF.
          ENDIF.
        ENDLOOP.
        IF lv_zero_quantitiy = 'X'.
          EXIT.
        ENDIF.
      ENDLOOP.
      lo_delivery->set_items( im_value = lt_items ).
    ENDLOOP.
```

## Add export control number
```
    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_items) = lo_delivery->get_items( ).
      LOOP AT lt_items INTO data(lo_item).
        data(lt_cls) = lo_item->get_classifications( ).
        data(lo_eccode) = im_cons_data_object_factory->new_aes_pb_cls_do(
                            im_type  = 'ECCODE'
                            im_value = 'LISTED'
                          ).
        APPEND lo_eccode to lt_cls.
        lo_item->set_classifications( im_value = lt_cls ).
      ENDLOOP.
    ENDLOOP.
```

## Change goods description for DE
Delete existing goods description for DE, create a new one and set it for the item
```
    DATA: lv_text TYPE /aeb/if_aes_pb_text_in_lan_do=>t_text.

    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_items) = lo_delivery->get_items( ).
      LOOP AT lt_items INTO DATA(lo_item).
        DATA(lt_goods_desc) = lo_item->get_goods_description( ).
        LOOP AT lt_goods_desc INTO DATA(lo_goods_desc).
          IF lo_goods_desc->get_language_iso_code( ) = 'DE'.
            DELETE lt_goods_desc.
          ENDIF.
        ENDLOOP.
        lv_text = 'neue Beschreibung'.
        lo_goods_desc = im_cons_data_object_factory->new_aes_pb_text_in_lan_do(
                          im_language_iso_code = 'DE'
                          im_text              = lv_text
                        ).
        APPEND lo_goods_desc TO lt_goods_desc.
        lo_item->set_goods_description( im_value = lt_goods_desc ).
      ENDLOOP.
    ENDLOOP.
```

## Set free template criteria on header level
```
    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_items) = lo_delivery->get_items( ).
      LOOP AT lt_items INTO DATA(lo_item).
        DATA(lt_free_temp_crits) = lo_item->get_free_template_criterias( ).
        DATA(lo_free_temp_crit) = im_cons_data_object_factory->new_aes_pb_free_temp_crit_do( ).
        lo_free_temp_crit->set_value( im_value = ls_vbak-augru && '_' && ls_vbak-auart ).
        APPEND lo_free_temp_crit TO lt_free_temp_crits.
        lo_item->set_free_template_criterias( im_value = lt_free_temp_crits ).
      ENDLOOP.
    ENDLOOP.
```

## Add a new partner (e.g. carrier)
```
    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_parties) = lo_delivery->get_parties( ).
      data(lo_carrier) = im_cons_data_object_factory->new_aes_pb_party_do(
                           im_company_number  = '1235'
                           im_party_type      = 'CARRIER'
                           im_country         = 'CH'
                         ).
      APPEND lo_carrier to lt_parties.
      lo_delivery->set_parties( im_value = lt_parties ).
    ENDLOOP.
```

## Add delivery number as client specific field
```
    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_items) = lo_delivery->get_items( ).
      LOOP AT lt_items INTO DATA(lo_item).
        DATA(lo_client_spec_fields) = lo_item->get_client_specific_fields( ).
        IF lo_client_spec_fields IS INITIAL.
          lo_client_spec_fields = im_cons_data_object_factory->new_aes_pb_clsp_fields_do( ).
          lo_item->set_client_specific_fields( im_value = lo_client_spec_fields ).
        ENDIF.
        DATA(lt_client_spec_fields) = lo_client_spec_fields->get_fields( ).
        DATA(lo_delivery_no) = im_cons_data_object_factory->new_aes_pb_clsp_field_do( ).
        lo_delivery_no->set_ident_code( im_value = 'DELIVERY_NO' ).
        lo_delivery_no->set_value( im_value = '1234' ).
        APPEND lo_delivery_no TO lt_client_spec_fields.
        lo_client_spec_fields->set_fields( im_value = lt_client_spec_fields ).
      ENDLOOP.
    ENDLOOP.
```

## Correct net weight for item
```
    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      data(lt_items) = lo_delivery->get_items( ).
      LOOP AT lt_items INTO DATA(lo_item).
        "Determine net weight here
        data(lo_net_weight_nv) = im_nullable_value_factory->dec_17_3( im_value = '1' ).
        data(lo_net_weight) = im_cons_data_object_factory->new_aes_pb_quantity_do_v1(
                                im_unit  = 'kg'
                                im_value = lo_net_weight_nv
                              ).
        lo_item->set_net_mass( im_value = lo_net_weight ).
      ENDLOOP.
    ENDLOOP.
```

## Set a consignment ID via additional references
```
    data(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      "Determine original invoice number here, e.g. via order flow or just reading VBRP-VGBEL
      DATA(lt_add_refs) = lo_delivery->get_additional_references( ).
      data(lo_cons_id) = im_cons_data_object_factory->new_aes_pb_add_ref_do(
                           im_reference_type = 'CONSIGNMENT_ID'
                           im_reference      = '123' "Enter original invoice here
                         ).
      APPEND lo_cons_id to lt_add_refs.
      lo_delivery->set_additional_references( im_value = lt_add_refs ).
    ENDLOOP.
```

## Add postcode information to consignee
```
    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_parties) = lo_delivery->get_parties( ).
      LOOP AT lt_parties INTO DATA(lo_party).
        IF lo_party->get_party_type( ) = 'CONSIGNEE' AND lo_party->get_post_code( ) IS INITIAL.
          lo_party->set_post_code( im_value = '.' ).
        ENDIF.
      ENDLOOP.
    ENDLOOP.
```

## Add produced documents (example for EXPORT_DE customs process)
```
    DATA: lo_de_itm_ext TYPE REF TO /aeb/if_aes_pb_ditm_de_do
          .

    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_items) = lo_delivery->get_items( ).
      LOOP AT lt_items INTO DATA(lo_item).
        DATA(lo_itm_ext) = lo_item->get_extension( ).
        IF lo_itm_ext IS INITIAL.
          lo_itm_ext = im_cons_data_object_factory->new_aes_pb_ditm_extd_do( ).
          lo_item->set_extension( im_value = lo_itm_ext ).
          lo_de_itm_ext = im_cons_data_object_factory->new_aes_pb_ditm_de_do( ).
          lo_itm_ext->set_export_de( im_value = lo_de_itm_ext ).
        ELSE.
          lo_de_itm_ext = lo_itm_ext->get_export_de( ).
          IF lo_de_itm_ext IS INITIAL.
            lo_de_itm_ext = im_cons_data_object_factory->new_aes_pb_ditm_de_do( ).
            lo_itm_ext->set_export_de( im_value = lo_de_itm_ext ).
          ENDIF.
        ENDIF.
        DATA(lt_prod_docs) = lo_de_itm_ext->get_produced_documents( ).
        DATA(lo_prod_doc) = im_cons_data_object_factory->new_aes_pb_prod_doc_eu_do( ).
        DATA(lo_type) = im_nullable_value_factory->char_9( im_value = 'Y901' ).
        lo_prod_doc->set_type_code( im_value = lo_type ).
        APPEND lo_prod_doc TO lt_prod_docs.
        lo_de_itm_ext->set_produced_documents( im_value = lt_prod_docs ).
      ENDLOOP.
    ENDLOOP.
```

## Set internal reference
```
im_value->set_internal_reference( im_value = 'Internal Reference' ).
```

## Initialize customs procedures on item level (e.g. for Swiss export in case you template the value on header level)
```
    DATA: lt_customs_procs TYPE /aeb/if_aes_pb_customs_proc_do=>tt_customs_proc_do
          .

    DATA(lt_deliveries) = im_value->get_deliveries( ).
    LOOP AT lt_deliveries INTO DATA(lo_delivery).
      DATA(lt_items) = lo_delivery->get_items( ).
      LOOP AT lt_items INTO DATA(lo_item).
        lo_item->set_customs_procedures( im_value = lt_customs_procs ).
      ENDLOOP.
    ENDLOOP.
```
