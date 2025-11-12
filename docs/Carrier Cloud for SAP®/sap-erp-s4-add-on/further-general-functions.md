---
title: Further general functions
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
## BAdIs to handle result of a shipping order update

To handle the result of the creation or every update of a shipping order, there are two BAdIs: 

1. Result handling with GUI:  /AEB/PA_SHP_RES_01 
2. Result handling in the background (no GUI): /AEB/PA_SHP_RES_02

<br />

Some example scenarios for the first mentioned BAdI (trigger is a GUI based interaction):

- Selecting the "Print labels" button from the shipping order monitor.    
- Use the "New with reference" function in the  shipping order monitor.

<br />

Some example scenarios for the second mentioned BAdI (trigger is coming from background processing):

- Trigger the data transfer to Carrier Cloud with an output message. 
- Trigger the data transfer in a background job 

<br />

## Get master data from Carrier Cloud for SAP

Sometimes it might be helpful to get master data from Carrier Cloud, e.g. codes of service types.  Use this function module for that purpose:  /AEB/PA_PB_GET_CAR_PROPS

***

## Write custom log entries

Using methods of the public API class /AEB/CL_PA_PB_CARRIER_BF will not write any log entries. But you can use the class /AEB/CL_PA_PB_LOGGER_BC  to write a log entry, e.g. to log the response data from the call.  

If the result contains an error or warning, you can write also an according log entry: 

> IF result->has_error( ) = 'X'. logger->create_log_error( ). ELSEIF result->has_warning( ) ...

<br />

***

## Define a different mail recipient

Use this BAdI to define a different mail recipient  when receiving a shipping order back in SAP:  /AEB/PA_SHP_DLV_03.  Requires setup  of mails using the distribution framework of the add-on:  [Mail distribution](https://rz3.aeb.de/docudata/inst-config-guides/carrier-connect/installconfig-carrierconnect-plug-insap/en-US/index.html#384091787665541771)

***

## Change the interface profile for TM based business objects

The add-on collects certain data from TM business objects depending on the settings in the collector profile. This profile is assigned to the export interface in the configuration. If you have the requirement for selecting a different profile, you can use the BAdI /AE1/80_PROF_FRO_01  

The BAdI method CHG_PROFILE_ID_FOR_FREIGHT_ORD has the following parameters: 

- header data of the business object (TM freight order, freight booking or consignment)  
- organizational unit as determined by the add-on 
- an object containing the profile ID - this value can be changed if required

<br />

## Open shipping order application in Carrier Connect for a SAP document

Use the according class to open the shipping order from your SAP system:   

| SAP Object    | Public Class             |
| :------------ | :----------------------- |
| Delivery      | /AEB/CL_PA_PB_DLV_DLS_AC |
| Shipment      | /AEB/CL_PA_PB_SHP_DLS_AC |
| Freight order | /AE1/CL_PA_PB_FRO_DLS_AC |

Code examples:

```Text Open shipping order
DATA:
    likp        TYPE likp,
    ac_delivery TYPE REF TO /AEB/CL_PA_PB_DLV_DLS_AC.

"Shipping order based on a SAP delivery
SELECT SINGLE * FROM likp INTO likp WHERE vbeln = '0080000123'.
ac_delivery = /AEB/CL_PA_PB_DLV_DLS_AC=>new_for( im_likp = likp ).
ac_delivery->open_shipping_order( ).


DATA:
    tor_no          TYPE /SCMTMS/TOR_ID,
    torrot          TYPE /SCMTMS/D_TORROT,
    ac_freightorder TYPE REF TO /AE1/CL_PA_PB_FRO_DLS_AC.


tor_no = '6000000123'.

"Shipping order based on a SAP freight order
SELECT SINGLE * FROM /SCMTMS/D_TORROT INTO torrot WHERE tor_id = tor_no.
ac_freightorder = /AE1/CL_PA_PB_FRO_DLS_AC=>new_for( im_torrot = torrot ).
ac_freightorder->open_shipping_order( ).


```