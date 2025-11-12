---
title: Adding country specific extensions
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
Some data is specific per country declaration procedure. Here are some examples for that.  

## EU specific extension on delivery level

```Text Adding deferred payments to a EU delivery
DATA:
      delivery       TYPE REF TO /aeb/if_aes_pb_delivery_do,
      extension      TYPE REF TO /aeb/if_aes_pb_dlv_extd_do,
      time_and_zone  TYPE REF TO /aeb/if_01_pb_date_and_zone_do,
      dlv_eu_do      TYPE REF TO /aeb/if_aes_pb_dlv_eu_do,
      location       TYPE REF TO /aeb/if_aes_pb_location_do,
      def_payments   TYPE /aeb/if_aes_pb_def_payment_do=>tt_def_payment_do,
      def_payment    TYPE REF TO /aeb/if_aes_pb_def_payment_do.
      
extension = im_cons_data_object_factory->new_aes_pb_dlv_extd_do( ).
dlv_eu_do = im_cons_data_object_factory->new_aes_pb_dlv_eu_do( ).
      
def_payment = im_cons_data_object_factory->new_aes_pb_def_pay_do( ).
def_payment->set_category_code( im_nullable_value_factory->char_1( 'F' ) ).
def_payment->set_deferral_account_number( im_nullable_value_factory->char_7( 'A123456' ) ).
def_payment->set_type_code( im_nullable_value_factory->char_1( 'A' ) ).
APPEND def_payment TO def_payments.      
dlv_eu_do->set_deferred_payments( def_payments ).

dlv_eu_do->set_departure_airport_code( 'FFM' ).
time_and_zone = im_cons_data_object_factory->new_01_pb_date_and_zone_do( im_date_in_timezone = '10110110110110.1101101'
    																																	   im_timezone = 'CET' ).
dlv_eu_do->set_expected_arrival_time( time_and_zone ).
dlv_eu_do->set_expected_departure_time( time_and_zone ).
      
location = im_cons_data_object_factory->new_aes_pb_location_do( ).
location->set_city( im_nullable_value_factory->char_35( 'Stuttgart' ) ).
location->set_country( im_nullable_value_factory->char_2( 'DE' ) ).
location->set_name( im_nullable_value_factory->char_120( 'HQ' ) ).
location->set_customs_process( im_nullable_value_factory->char_20( 'EXPORT_BE' ) ).
dlv_eu_do->set_goods_location( location ).
      
extension->set_export_be( dlv_eu_do ).
delivery->set_extension( extension ).
```

## GB GVMS extension

```text adding a gvms gb extension to a delivery
DATA:
      delivery       TYPE REF TO /aeb/if_aes_pb_delivery_do,
      extension      TYPE REF TO /aeb/if_aes_pb_dlv_extd_do,
      time_and_zone  TYPE REF TO /aeb/if_01_pb_date_and_zone_do,
      gvms_gb        TYPE REF TO /aeb/if_aes_pb_dlv_gvms_do.

extension = im_cons_data_object_factory->new_aes_pb_dlv_extd_do( ).
gvms_gb = im_cons_data_object_factory->new_aes_pb_gvms_gb_do( ).
gvms_gb->set_is_unaccompanied( 'X' ).
gvms_gb->set_planned_route_id( 'route id' ).
time_and_zone = im_cons_data_object_factory->new_01_pb_date_and_zone_do( im_date_in_timezone = '10110110110110.1101101'
im_timezone = 'CET' ).
gvms_gb->set_planned_departure_date( time_and_zone ).
extension->set_gmvs_gb( gvms_gb ).
delivery->set_extension( extension ).
```

## DE specific extension on delivery level

```Text Adding a summary declaration DE to a delivery
DATA:
      delivery       TYPE REF TO /aeb/if_aes_pb_delivery_do,
      extension      TYPE REF TO /aeb/if_aes_pb_dlv_extd_do,
      sum_decl       TYPE REF TO /aeb/if_aes_pb_dlv_s_dec_do,
      time_and_zone  TYPE REF TO /aeb/if_01_pb_date_and_zone_do,
      previous_doc   TYPE REF TO /aeb/if_aes_pb_dlv_prev_doc_do.

sum_decl = im_cons_data_object_factory->new_aes_pb_sum_doc_do( ).
time_and_zone = im_cons_data_object_factory->new_01_pb_date_and_zone_do( im_date_in_timezone = '10110110110110.1101101'
im_timezone = 'CET' ).
sum_decl->set_arrival_date( time_and_zone ).
sum_decl->set_presentation_date( time_and_zone ).
sum_decl->set_container_quantity( 234 ).
previous_doc = im_cons_data_object_factory->new_aes_pb_prev_doc_do( ).
previous_doc->set_type( 'type' ).
previous_doc->set_category( 'A' ).
previous_doc->set_reference( '123456789' ).
previous_doc->set_complementary_info( 'complementary info' ).
sum_decl->set_previous_document( previous_doc ).
extension->set_sum_decl_de( sum_decl ).
delivery->set_extension( extension ).
```

## DE specific extension on item level

```text Delivery item export DE extension with previous documents, produced documents and preferential data
DATA:
      delivery       TYPE REF TO /aeb/if_aes_pb_delivery_do,
      item           TYPE REF TO /aeb/if_aes_pb_delivery_itm_do,
      previous_doc   TYPE REF TO /aeb/if_aes_pb_dlv_prev_doc_do,
      previous_docs  TYPE /aeb/if_aes_pb_dlv_prev_doc_do=>tt_prev_doc_do,
      produced_doc   TYPE REF TO /aeb/if_aes_pb_prod_doc_eu_do,
      produced_docs  TYPE /aeb/if_aes_pb_prod_doc_eu_do=>tt_prod_doc_eu_do,
      pref_data      TYPE REF TO /aeb/if_aes_pb_pref_data_eu_do,
      itm_ext        TYPE REF TO /aeb/if_aes_pb_ditm_extd_do,
      ditm_exp_de_do TYPE REF TO /aeb/if_aes_pb_ditm_de_do.
      
READ TABLE delivery->get_items( ) INTO item INDEX 1.
itm_ext = im_cons_data_object_factory->new_aes_pb_ditm_extd_do( ).
ditm_exp_de_do = im_cons_data_object_factory->new_aes_pb_ditm_de_do( ).
ditm_exp_de_do->set_origin_country_region_code( im_nullable_value_factory->char_4( 'GER0' ) ).

" preferential data
pref_data = im_cons_data_object_factory->new_aes_pb_pref_data_eu_do( ).
pref_data->set_preference_type( im_nullable_value_factory->char_3( 'pre' ) ).
ditm_exp_de_do->set_preferential_data( pref_data ).

" previous documents
previous_doc = im_cons_data_object_factory->new_aes_pb_prev_doc_do( ).
previous_doc->set_type( 'type' ).
previous_doc->set_category( 'A' ).
previous_doc->set_reference( '123456789' ).
previous_doc->set_complementary_info( 'complementary info' ).
APPEND previous_doc TO previous_docs.
ditm_exp_de_do->set_previous_documents( previous_docs ).

" produced documents
produced_doc = im_cons_data_object_factory->new_aes_pb_prod_doc_eu_do( ).
produced_doc->set_complementary_info( im_nullable_value_factory->char_26( 'complementary info' ) ).
APPEND produced_doc TO produced_docs.
ditm_exp_de_do->set_produced_documents( produced_docs ).

itm_ext->set_export_de( ditm_exp_de_do ).
item->set_extension( itm_ext ).
```