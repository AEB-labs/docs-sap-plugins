---
title: Change the data for Export Controls checks
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: 'How to trigger questionnaires for a risk assessment process:'
  pages:
    - type: basic
      slug: creating-questionnaires-for-risk-assessment
      title: Creating questionnaires for Risk Assessment
---
## Overview

You can adapt the data used in the Export Controls check. Just implement the BAdI for each business object accordingly: 

| Data object                    | BAdI                 |
| :----------------------------- | :------------------- |
| Sales document                 | /AEB/CMP_EC_ORDER_06 |
| Delivery                       | /AEB/CMP_EC_DLV_03   |
| Purchase document              | /AEB/CMP_EC_PD_03    |
| Service order (ERP)            | /AEB/CMP_EC_SO_03    |
| Service transactions (S/4HANA) | /AE1/CMP_EC_ST_02    |

## Data structure

Before you start the implementation, take a look on the data structures used in the Export Controls check:

![](https://files.readme.io/6b22cef-2020-02-07_125021.jpg "2020-02-07_125021.jpg")

The whole transaction is provided in an object-oriented structure. To access the relations and fields, use the given get/set/add -methods.  
A _transaction_ is the checked document, i.e. a sales order.  Linked to that are the according _items_ i.e. sales order items. For Trade Compliance Management all checks are executed on item level, which also hold the classification values such as the _Export Control Number_

## Concept explanation: How to change the item quantity

Let's start with a simple implementation to change the quantity on item level:

```text Change quantity of item
DATA:
    items         TYPE /aeb/if_cmp_pb_ec_item_do=>tt_ec_item_do,
    curr_item     TYPE REF TO /aeb/if_cmp_pb_ec_item_do,
    quantity type TYPE REF TO /AEB/CL_01_DEC_15_3_NV.
    
items = im_value->get_items( ).
loop at items into curr_item.
	quantity = im_nullable_value_factory->dec_15_3( '1' ).
	curr_item->set_quantity( quantity ).
endloop.
```

Explanation of  this code snippet: 

- First, we access the items by calling the method _get_items_. The result ist a list of instances of type interface /aeb/if_cmp_ec_item_do.
- Then we loop over these instances. The quantity and any other plain field is provided as a so called  "nullable value". We use the nullable values to differ between "null" and "0.0". 
- To create such nullable value use the factory parameter \_im_nullable_value_factory_. This parameter has methods to create each type that is supported.
- In our example we use the type dec_15_3._ After you have assigned the quantity value to the according variable, you can apply it to the item by using the set-method_ \_set_quantity_.

## Using the context instead of reading the database

Ok now let us extend the example by using a z-field of table VBAP to set the quantity: 

```text Find the right vbap for the z-field quantity
DATA:
    items         TYPE /aeb/if_cmp_pb_ec_item_do=>tt_ec_item_do,
    curr_item     TYPE REF TO /aeb/if_cmp_pb_ec_item_do,
    quantity type TYPE REF TO /AEB/CL_01_DEC_15_3_NV,
   	item_ref TYPE REF TO /AEB/CL_01_CHAR_255_NV,
    posnr type posnr,
    vbaps type /AEB/CMP_PB_VBAPS,
    curr_vbap type vbap.
    
items = im_value->get_items( ).
loop at items into curr_item.
	item_ref = curr_item->get_reference_id( ).
  posnr = item_ref->v.
  vbaps = im_Context_bc->get_vbaps( ).
  read table vbaps into curr_vbap with key posnr = posnr.
  
	quantity = im_nullable_value_factory->dec_15_3( curr_vbap-z_quantity ).
	curr_item->set_quantity( quantity ).
endloop.
```

As you can see, we don't read the vbap from the database, and rather get it from the context object using the method _get_vbaps_. To find the right item, we use the reference id from the curr_item. 

## Changing classifications

This example shows how to overwrite the export control number for the EU jurisdiction: 

```text Overwrite EC number
DATA:
    items         TYPE /aeb/if_cmp_pb_ec_item_do=>tt_ec_item_do,
    curr_item     TYPE REF TO /aeb/if_cmp_pb_ec_item_do,
   	item_ref      TYPE REF TO /AEB/CL_01_CHAR_255_NV,
    classification   TYPE REF TO /aeb/if_cmp_pb_ec_prd_cls_do,
    classifications TYPE /AEB/IF_CMP_PB_EC_PRD_CLS_DO=>TT_EC_PRODUCT_CLASSIFICATION,
    classification_code TYPE REF TO /AEB/CL_01_CHAR_50_NV,
    classification_number TYPE REF TO /AEB/CL_01_CHAR_50_NV,
    posnr type posnr,
    vbaps type /AEB/CMP_PB_VBAPS,
    curr_vbap type vbap.
    
items = im_value->get_items( ).
loop at items into curr_item.
	item_ref = curr_item->get_reference_id( ).
  posnr = item_ref->v.
  vbaps = im_Context_bc->get_vbaps( ).
  read table vbaps into curr_vbap with key posnr = posnr.
  
  classifications = curr_item->get_product_classifications( ).
  loop at classifications into classification.
  	classification_code = classification->GET_CLASSIFICATION_IDENTCODE( ).
    if classification_code->v = 'ClassificationAusfuhrliste'.
    	classification_number = im_nullable_value_factory->char_50( curr_vbap-z_ecn ).
      classification->set_classification_number( classification_number ).
    endif.
  endloop.	
endloop.
```

This example outlines that you just need to identify the according classification and then update it. But it's not neccessary to update the whole item or the transaction. 

## Adding classifications

You can as well add classifications. The following example does that: 

```text Add new product classification
DATA:
  items               TYPE /aeb/if_cmp_pb_ec_item_do=>tt_ec_item_do,
  curr_item           TYPE REF TO /aeb/if_cmp_pb_ec_item_do,
  item_ref            TYPE REF TO /aeb/cl_01_char_255_nv,
  classification      TYPE REF TO /aeb/if_cmp_pb_ec_prd_cls_do,
  classifications     TYPE /aeb/if_cmp_pb_ec_prd_cls_do=>tt_ec_product_classification,
  classification_code TYPE REF TO /aeb/cl_01_char_50_nv,
  classification_no   TYPE REF TO /aeb/cl_01_char_50_nv,
  posnr               TYPE posnr,
  vbaps               TYPE /aeb/cmp_pb_vbaps,
  curr_vbap           TYPE vbap.
    
items = im_value->get_items( ).
loop at items into curr_item.
  item_ref = curr_item->get_reference_id( ).
  posnr = item_ref->v.
  vbaps = im_Context_bc->get_vbaps( ).
  read table vbaps into curr_vbap with key posnr = posnr.  
  classification = im_data_object_factory->new_cmp_pb_ec_prd_cls_do( ).
  classification_code = im_nullable_value_factory->char_50( 'ADD_CODE_1' ).
  classification->set_classification_identcode( classification_code ).
  classification_no = im_nullable_value_factory->char_50( curr_vbap-add_code_1_val ).
  classification->set_classification_number( classification_no ).  
  curr_item->add_product_classification( classification ).
endloop.
```

Use the "add"-method to add a new classification. To create an instance for a new classificatio, use the method "new_cmp_pb_ec_prd_cls_do" of the provided parameter "im_data_object_factory".

## Get the values based on profiles maintained in Product Classification

And now we like to use Product Classification to overwrite the classifications by profile to the items. 

```text Use Product Classification service to overwrite classifications
DATA:
  items               TYPE /aeb/if_cmp_pb_ec_item_do=>tt_ec_item_do,
  curr_item           TYPE REF TO /aeb/if_cmp_pb_ec_item_do,
  classifications     TYPE /aeb/if_cmp_pb_ec_prd_cls_do=>tt_ec_product_classification.
    
items = im_value->get_items( ).
loop at items into curr_item.
   classifications = im_classification_bc->get_classifications_for( 
   		im_classification_profile = 'CUS_CLS_PROFILE' "this profile has to exist in Product Classification
        im_ec_item = item ).
                
   curr_item->set_product_classifications( classifications ).   
endloop.
```

## Read classifications with date today

In some cases you might want  to read the value of a classification.  This can be achieved like this:  

```markdown Read classifications with decisive date = today
DATA:
    classifications        TYPE /aeb/if_cmp_pb_ec_prd_cls_do=>tt_ec_product_classification,
    standard_decisive_date TYPE REF TO /aeb/cl_01_dats_nv.
  
  LOOP AT im_value->get_items( ) INTO item.
  *save the original decisive date first so it can be restored later for the item
  standard_decisive_date = item->get_decisive_date( ).
  *always use date today to read classifications 
    item->set_decisive_date( im_nullable_value_factory->dats( sy-datum ) ).
    classifications = im_classification_bc->get_classifications_for(
        im_ec_item                = item
        im_classification_profile = 'Choose the right profile code here'  ). 
    item->set_product_classifications( classifications ).
   *restore the decisive date 
    item->set_decisive_date( standard_decisive_date ).
  ENDLOOP.
```

## Manage item values

If you have the requirement to overwrite the values you must keep in mind that whenever you change the document value you also should update the plugin values (also called jurisdiction values), which simply represent the same value converted to to amounts in different currencies. 

These additional amounts per currency are relevant, for example, if licenses are setup in a currency different to the document currency. In general, the document currency combined with the plug-in values results in a list of all currencies that are transferred to Trade Compliance Management. Each currency may only appear once here. This list is transferred in the check as valueOfGoods. When a license is created, the document currency is transferred as the original value and the appropriate currency for the jurisdiction of the license is determined and also transferred. All possibly required currencies for the creation of the licenses must therefore be available.

So let's update the document values and also the additional jurisdiction value for EU-jurisdiction (here we need the value in EUR currency). For our next case the value we like to use as document value is again stored in a z-field of table vbap. 

```text Change document value and plugin values
DATA:
      items            TYPE /aeb/if_cmp_pb_ec_item_do=>tt_ec_item_do,
      curr_item        TYPE REF TO /aeb/if_cmp_pb_ec_item_do,
      item_ref         TYPE REF TO /aeb/cl_01_char_255_nv,
      posnr            TYPE posnr,
      vbaps            TYPE /aeb/cmp_pb_vbaps,
      curr_vbap        TYPE vbap,
      amount_of_money  TYPE REF TO /aeb/if_cmp_pb_ec_aom_do,
      value            TYPE REF TO /aeb/cl_01_dec_15_2_nv,
      amount_of_moneys TYPE /aeb/if_cmp_pb_ec_aom_do=>tt_ec_amount_of_money.

    items = im_value->get_items( ).
    LOOP AT items INTO curr_item.
      item_ref = curr_item->get_reference_id( ).
      posnr = item_ref->v.
      vbaps = im_context_bc->get_vbaps( ).
      READ TABLE vbaps INTO curr_vbap WITH KEY posnr = posnr.

      amount_of_money = curr_item->get_document_value( ).
      value = im_nullable_value_factory->dec_15_2( curr_vbap-z_doc_value ).
      amount_of_money->set_value( value ).

      amount_of_moneys = curr_item->get_plugin_values( ).
      LOOP AT amount_of_moneys INTO amount_of_money.
        IF amount_of_money->get_currency_iso( )->v = 'EUR'.
          value = im_nullable_value_factory->dec_15_2( curr_vbap-z_doc_value_in_eur ).
          amount_of_money->set_value( value ).
        ENDIF.
      ENDLOOP.
    ENDLOOP.
```

Make sure that you do not include amounts in the same currency twice in document value and in plugin value: The document currency must not be included in the plug-in currency. Also, if you add a plugin value the added currency must not already exist.

## Add or change partner information

Another usecase might be that you like to add an addtional partner, because you have this partner information in Z-Fields of the VBAK table,e.g. the end user. In this example we assume that the standard won't fill the end user. 

```text add partner to the transaction
DATA:
      partner  TYPE REF TO /aeb/if_cmp_pb_ec_partner_do,
      prt_code TYPE REF TO /aeb/cl_01_char_20_nv,
      name     TYPE REF TO /aeb/cl_01_char_40_nv,
      country  TYPE REF TO /aeb/cl_01_char_2_nv,
      vbak     TYPE vbak.
    
    vbak = im_context_bc->get_vbak( ).
    
    partner = im_data_object_factory->new_cmp_pb_ec_partner_do( ).
    
    prt_code = im_nullable_value_factory->char_20( 'ENDUSER_STD' ).
    name = im_nullable_value_factory->char_40( vbak-z_euser_name ).
    ctry = im_nullable_value_factory->char_2( vbak-z_euser_ctry ).
    
    partner->set_identcode( prt_code ).
    partner->set_name1( name ).
    partner->set_country_iso( country ).
    
    im_value->add_partner( partner ).
```
```text overwrite identcode of existing partner
DATA:
  curr_partner  TYPE REF TO /aeb/if_cmp_pb_ec_partner_do,
  curr_role     TYPE REF TO /aeb/cl_01_char_50_nv,
  new_identcode TYPE REF TO /aeb/cl_01_char_20_nv.

LOOP AT im_value->get_partners( ) INTO curr_partner.
  CASE curr_partner->get_role_identcode( )->v.
    WHEN 'CONSIGNOR_STD'.
      new_identcode = im_nullable_value_factory->char_20( 'Company no.' ).
      curr_partner->set_identcode( new_identcode ).
    WHEN 'SELLER_STD'.
      new_identcode = im_nullable_value_factory->char_20( 'Company no.' ).
      curr_partner->set_identcode( new_identcode ).
  ENDCASE.
ENDLOOP.
```

Again we use the im_context_bc to get data for the business object. Do not use database statemens for transactional data here. For the country you have to provide the country ISO code, not the country key.  
With the \_add_partner_ method you can easily add the partner to the transaction. In the same way it would also work for the items.

## Overwrite the completion status

Now let us overwrite the completion status of the EU-ExportControls No and the ECCN from item data.

> 🚧 Overwriting is is only necessary if "version 1" of the "Goods attribute handling" is activated. This can be checked in the TCM system --> Administration --> Compliance --> Settings (client specific) --> Tab "Export Controls" --> Group "Handling of goods attributes"

```text Overwrite completion status
DATA:
      items             TYPE /aeb/if_cmp_pb_ec_item_do=>tt_ec_item_do,
      curr_item         TYPE REF TO /aeb/if_cmp_pb_ec_item_do,
      item_ref          TYPE REF TO /aeb/cl_01_char_255_nv,
      compl_statuss     TYPE /aeb/if_cmp_pb_ec_pcst_do=>tt_ec_product_completion_sta,
      posnr             TYPE posnr,
      vbaps             TYPE /aeb/cmp_pb_vbaps,
      curr_vbap         TYPE vbap,
      curr_compl_status TYPE REF TO /aeb/if_cmp_pb_ec_pcst_do,
      is_complete       TYPE REF TO /aeb/cl_01_boolean_nv.
    items = im_value->get_items( ).
    LOOP AT items INTO curr_item.
      item_ref = curr_item->get_reference_id( ).
      posnr = item_ref->v.
      vbaps = im_context_bc->get_vbaps( ).
      READ TABLE vbaps INTO curr_vbap WITH KEY posnr = posnr.

      compl_statuss = curr_item->get_product_completion_status( ).
      LOOP AT compl_statuss INTO curr_compl_status.
        CASE curr_compl_status->get_plugin_identcode( )->v.
          WHEN 'DE-EU-Plugin'.
            IF curr_vbap-z_eu_sta = 'X'.
              is_complete = im_nullable_value_factory->boolean( 'X' ).
            ELSE.
              is_complete = im_nullable_value_factory->boolean( '-' ).
            ENDIF.
            curr_compl_status->set_is_product_complete( is_complete ). 
          WHEN 'US-EAR-Plugin'.
            IF curr_vbap-z_us_sta = 'X'.
              is_complete = im_nullable_value_factory->boolean( 'X' ).
            ELSE.
              is_complete = im_nullable_value_factory->boolean( '-' ).
            ENDIF.
            curr_compl_status->set_is_product_complete( is_complete ).
        ENDCASE.
      ENDLOOP.
    ENDLOOP.
```

## Exclude items from check

```markdown Exclude item
LOOP AT im_value->get_items( ) INTO item.
IF z_custom_logic_item_is_relevant( item ) = 'X'. "implement your own z-logic here  
   item_ref_id = item->get_reference_id( ).  
   im_value->delete_item_for( item_ref_id ).  
ENDIF.  
ENDLOOP

```