---
title: BAdIs to change the transfered data
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
## General process

The add-on logic determines certain data based on the SAP business object and transforms it into the interface structure as expected by the Carrier Cloud API.  It's possible to influence this logic and also change the determined data using the BAdIs listed below. Eventually a shipment is created in Carrier Cloud.

Technical process:

1. The data transfer is triggered for an SAP business object, e.g. an outbound delivery.
2. Prior to the data determination the add-on runs through the BAdI with method BEFORE_STD_FILLING
3. The add-on collects data based on the standard logic. This logic is depended on the configured rules and, if implemented, the changes from step #2
4. After the data determination, the add-on runs through the BAdI with method AFTER_STD_FILLING.
5. The Carrier Cloud API is called to send the determined data.
6. Carrier cloud creates a shipment based on the data and triggers further actions, e.g. label creation.

<br />

## BEFORE_STD_FILLING -  prior to data determination

These BAdIs will be run through prior to the determination of data.  Deviating partner functions can be stored for the document partners. The actual determination process pulls the data from the newly assigned partner role.

| SAP document                             | BAdI                                                                              |
| :--------------------------------------- | :-------------------------------------------------------------------------------- |
| Delivery (SD)                            | /AEB/PA_SHP_DLV_01 - BEFORE_STD_FILLING                                           |
| Shipment (LE_TRA)                        | /AEB/PA_SHP_SHP_01 - BEFORE_STD_FILLING                                           |
| Freight order, booking, consignment (TM) | There is no designated BAdI for this step. Use the profile configuration instead. |

Parameters:

IM_ORG_UNIT (organizational unit)<br />IM_CONTEXT_BC (data context of the SAP document)<br />CH_PARVW_CONSIGNEE (partner role for consignee)<br />CH_PARVW_CUSTOMSAGENT (partner role for customs agent)<br />CH_PARVW_COVERADDRESS (partner role for cover address)<br />CH_PARVW_NOTIFYADDRESS (partner role for notify address)<br />CH_PARVW_PAYEROFCHARG (partner role for freight payer)<br />CH_PARVW_PAYEROFDUT (partner role for payer of customs duties)<br />CH_PARVW_PICKUPADDRESS (partner role for the pickup address)<br />CH_PARVW_ALT_INV_RECIPIENT (partner role for invoice recipient)

## AFTER_STD_FILLING - after data determination

These BAdIs will be run through at the end of the data determination process . All fields can be changed as required before the API for the data transfer is called.

| SAP document                             | BAdI                                   |
| :--------------------------------------- | :------------------------------------- |
| Delivery (SD)                            | /AEB/PA_SHP_DLV_02 - AFTER_STD_FILLING |
| Shipment (LE-TRA)                        | /AEB/PA_SHP_SHP_02 - AFTER_STD_FILLING |
| Freight order, booking, consignment (TM) | /AE1/PA_SHP_FRO_01 - AFTER_STD_FILLING |

<br />

## Implementation examples

### Change the content

<br />

Assign contact details of the consignee

```text Contact details
ch_shp_req_do-consigneecontact-name = 'ACME Receiving '.
ch_shp_req_do-consigneecontact-phone = '+4412035633'.
ch_shp_req_do-consigneecontact-email = 'receiving@acme-group.com'.

```

<br />

<br />

<br />

### Assign a reference text

Here is an implementation example for BAdI /AEB/PA_SHP_DLV_02 that assigns a value to the reference text  with the reference type "delivery note number":

```text Assign a reference text
METHOD /aeb/if_ex_pa_shp_dlv_02~after_std_filling.
  DATA:
    ref_text TYPE REF TO /aeb/if_pa_pb_dl_ref_text_do.

  ref_text = im_data_object_factory->new_pa_pb_dl_ref_text_do(
               im_type  = /aeb/if_pa_pb_dl_ref_text_do=>c_type_delnote_number
               im_value = '87600123' ).
  APPEND ref_text TO ch_shp_req_do-referencestexts.
ENDMETHOD.
```

<br />

### Include fields for additional values

```text Additional field
DATA:
  add_field TYPE /aeb/01_pb_gen_df_do.
  
  add_field-name = 'CODE23'.  
  add_field-type = 'String'.   
  add_field-value = 'This is the value'.
  
  APPEND add_field TO ch_shp_req_do-additionalvalues-fields.
```

<br />

### Change earliest requested delivery date

A couple of fields can be set in the BAdI using the interface DATA of type /AEB/IF_PA_PB_DL_SHP_REQ_DO.  When working with this, use the according set-methods or get methods.

Example: Set the field for the earliest requested delivery date

```text Earliest req. date
 ch_shp_req_do-data->set_earliest_requested_dlv_dat( 
        im_nullable_value_factory->char_19( '20220101121212' )  ).  

```
