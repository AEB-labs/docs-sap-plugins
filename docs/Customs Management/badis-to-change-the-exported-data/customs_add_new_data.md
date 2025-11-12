---
title: Add new data
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
      slug: badi-aebaes_cons_sdv_01
      title: BAdI /AEB/AES_CONS_SDV_01
---
## Add an extension on item level for a declaration procedure

There a different kind of extensions per country. This example shows how to add an item extension for Sweden. 

```text Add an extension on item level for a specific declaration procedure (e.g. Export SE)
DATA:
    dlv_dos      TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,
    dlv_do       TYPE REF TO /aeb/if_aes_pb_delivery_do,
    itm_dos      TYPE /aeb/if_aes_pb_delivery_itm_do=>tt_delivery_item_do,
    itm_do       TYPE REF TO /aeb/if_aes_pb_delivery_itm_do,
    itm_ext_do   TYPE REF TO /aeb/if_aes_pb_ditm_extd_do,
    itm_exp_se   TYPE REF TO /aeb/if_aes_pb_ditm_eu_do,
    pref_data    TYPE REF TO /aeb/if_aes_pb_pref_data_eu_do,
    prod_doc_dos TYPE /aeb/if_aes_pb_prod_doc_eu_do=>tt_prod_doc_eu_do,
    prod_doc_do  TYPE REF TO /aeb/if_aes_pb_prod_doc_eu_do.

  dlv_dos = im_value->get_deliveries( ).
  LOOP AT dlv_dos INTO dlv_do.
    itm_dos = dlv_do->get_items( ).
    LOOP AT itm_dos INTO itm_do.
*     Instantiate the extension of type Export SE
      itm_ext_do = im_cons_data_object_factory->new_aes_pb_ditm_extd_do( ).
      itm_do->set_extension( itm_ext_do ).
      itm_exp_se = im_cons_data_object_factory->new_aes_pb_ditm_eu_do( ).
      itm_ext_do->set_export_se( itm_exp_se ).

*     Fill the nature of transaction code
      itm_exp_se->set_transaction_nature_code( im_nullable_value_factory->char_2( '22' ) ).

*     Fill the preferential data
      pref_data = im_cons_data_object_factory->new_aes_pb_pref_data_eu_do( ).
      itm_exp_se->set_preferential_data( pref_data ).
      pref_data->set_preference_type( im_nullable_value_factory->char_3( '333' ) ).

*     Add a document
      prod_doc_do = im_cons_data_object_factory->new_aes_pb_prod_doc_eu_do( ).
      prod_doc_do->set_amount( im_cons_data_object_factory->new_aes_pb_aom_do_v1( 
        im_currency_iso = 'EUR'
        im_value        = im_nullable_value_factory->dec_17_2( '5000.00' ) ) ).
      prod_doc_do->set_complementary_info( im_nullable_value_factory->char_26( 'This is a info' ) ).
      prod_doc_do->set_document_part( im_nullable_value_factory->char_5( '10002' ) ).
      prod_doc_do->set_information_date( im_cons_data_object_factory->new_01_pb_date_and_zone_do( 
        im_date_in_timezone = '20190522120000'
        im_timezone         = 'UTC' ) ).
      prod_doc_do->set_quantity( im_cons_data_object_factory->new_01_quantity_do(
        im_unit  = 'ST'
        im_value = '1' ) ).
      prod_doc_do->set_reference( im_nullable_value_factory->char_35( '35000' ) ).
      prod_doc_do->set_status_code( im_nullable_value_factory->char_2( '22' ) ).
      prod_doc_do->set_type_code( im_nullable_value_factory->char_9( 'Y901' ) ).
      CLEAR prod_doc_dos.
      APPEND prod_doc_do TO prod_doc_dos.
      
*     Add a second document      
      prod_doc_do = im_cons_data_object_factory->new_aes_pb_prod_doc_eu_do( ).
      prod_doc_do->set_amount( im_cons_data_object_factory->new_aes_pb_aom_do_v1( 
        im_currency_iso = 'EUR'
        im_value        = im_nullable_value_factory->dec_17_2( '5000.00' ) ) ).
      prod_doc_do->set_complementary_info( im_nullable_value_factory->char_26( 'This is the second info' ) ).
      prod_doc_do->set_document_part( im_nullable_value_factory->char_5( '10002' ) ).
      prod_doc_do->set_reference( im_nullable_value_factory->char_35( '35000' ) ).
      prod_doc_do->set_type_code( im_nullable_value_factory->char_9( 'N380' ) ).
      APPEND prod_doc_do TO prod_doc_dos.
      itm_exp_se->set_produced_documents( prod_doc_dos ).
    ENDLOOP.
  ENDLOOP.
```

## Add equipment with container information

```text Setting the container number
DATA:
    dlv_dos          TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,
    dlv_do           TYPE REF TO /aeb/if_aes_pb_delivery_do,
    equipment_dos    TYPE /aeb/if_aes_pb_trsprt_equip_do=>tt_transport_equip_do,
    equipment_do     TYPE REF TO /aeb/if_aes_pb_trsprt_equip_do,
    container_number TYPE /aeb/if_aes_pb_trsprt_equip_do=>t_identification.

  dlv_dos = im_value->get_deliveries( ).
  LOOP AT dlv_dos INTO dlv_do.
    equipment_dos = dlv_do->get_transport_equipments( ).

    container_number = im_vttk-signi.
    equipment_do = im_cons_data_object_factory->new_aes_pb_trsprt_equip_do( im_equipment_type = 'CONTAINER'
                                                                            im_identification = container_number ).
    APPEND equipment_do TO equipment_dos.

    dlv_do->set_transport_equipments( equipment_dos ).
  ENDLOOP.
```

## Add an additional reference for the port booking number

```text Setting the port booking number with the forwarding agent tracking ID
DATA:
    dlv_dos                  TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,
    dlv_do                   TYPE REF TO /aeb/if_aes_pb_delivery_do,
    additional_reference_dos TYPE /aeb/if_aes_pb_add_ref_do=>tt_add_reference_do,
    additional_reference_do  TYPE REF TO /aeb/if_aes_pb_add_ref_do,
    port_booking_number      TYPE /aeb/if_aes_pb_add_ref_do=>t_reference.

  dlv_dos = im_value->get_deliveries( ).
  LOOP AT dlv_dos INTO dlv_do.
    additional_reference_dos = dlv_do->get_additional_references( ).

    port_booking_number = im_vttk-tndr_trkid.
    additional_reference_do = im_cons_data_object_factory->new_aes_pb_add_ref_do( im_reference_type = 'PORT_BOOKING_NUMBER'
                                                                                    im_reference = port_booking_number ).
                                                                                    
		APPEND additional_reference_do TO additional_reference_dos.
    dlv_do->set_additional_references( additional_reference_dos ).
  ENDLOOP.
```

## Add client specific fields

Client specific specific fields can for example be used for template determination based on very specific fields, which are not part of the standard consignment. Client specific fields exist on delivery and item level. Note that the client specific fields have to be defined in the customs management engine. 

```
DATA:
    dlv_dos               TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,
    dlv_do                TYPE REF TO /aeb/if_aes_pb_delivery_do,
    client_specific_field TYPE REF TO /aeb/if_aes_pb_clsp_fld_do,
    fields                TYPE /aeb/if_aes_pb_clsp_fld_do=>tt_aes_pb_clnt_spec_fld_do,
    clnt_specific_fields  TYPE REF TO /aeb/if_aes_pb_clsp_flds_do.

  dlv_dos = im_value->get_deliveries( ).
  LOOP AT dlv_dos INTO dlv_do.
    client_specific_field = im_cons_data_object_factory->new_aes_pb_clsp_field_do( ).
    clnt_specific_fields = im_cons_data_object_factory->new_aes_pb_clsp_fields_do( ).
  
  	client_specific_field->set_customs_process( 'EXPORT_DE' ).
    client_specific_field->set_ident_code( 'DOC_TYPE' ).
    client_specific_field->set_value( 'DLV' ).
    APPEND client_specific_field TO fields.
    clnt_specific_fields->set_fields( fields ).
    dlv_do->set_client_specific_fields( clnt_specific_fields ).
  ENDLOOP.
```

## Add data for Customs Inventory Management

```
        DATA:
          dlv_dos                    TYPE /aeb/if_aes_pb_delivery_do=>tt_delivery_do,
          dlv_do                     TYPE REF TO /aeb/if_aes_pb_delivery_do,
          item_do                    TYPE REF TO /aeb/if_aes_pb_delivery_itm_do,
          customs_special_procedures TYPE REF TO /aeb/if_aes_pb_cus_spro_do,
          references_removal_do      TYPE REF TO /aeb/if_aes_pb_cus_sp_ref_do,
          customs_procedures         TYPE /aeb/if_aes_pb_customs_proc_do=>tt_customs_proc_do,
          customs_procedure          TYPE REF TO /aeb/if_aes_pb_customs_proc_do.

        customs_procedure = im_cons_data_object_factory->new_aes_pb_customs_proc_do( im_customs_procedure_code = im_nullable_value_factory->char_20( '7100' )
                                                                                                 im_customs_procedure_type = im_nullable_value_factory->char_35( 'IMPORTPROC' ) ).
        APPEND customs_procedure TO customs_procedures.

        dlv_dos = im_value->get_deliveries( ).
        LOOP AT dlv_dos INTO dlv_do.
          LOOP AT dlv_do->get_items( ) INTO item_do.
            customs_special_procedures = im_cons_data_object_factory->new_aes_pb_cus_spro_do( ).
            customs_special_procedures->set_identities_entry( VALUE #( ( 'IDENT1' ) ) ).
            references_removal_do = im_cons_data_object_factory->new_aes_pb_cus_sp_ref_do( ).
            references_removal_do->set_booking_request_id( '210ATH' ).
            customs_special_procedures->set_references_removal( VALUE #( ( references_removal_do ) ) ).
            item_do->set_customs_special_procedures( im_value = customs_special_procedures ).
            item_do->set_customs_procedures( im_value = customs_procedures ).
          ENDLOOP.
        ENDLOOP.
```
