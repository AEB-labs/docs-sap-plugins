---
title: Custom objects
deprecated: false
hidden: false
metadata:
  robots: index
---
The add-on provides functionality pre-defined for outbound deliveries, shipments, freight orders and sales orders. But apart from that you can also transfer any other documents from your SAP system. To support this, the add-on provides functions for a so called custom object or "any objects" as follows.&#x20;

## Transfer a custom object&#x20;

Function module /AEB/MA_PB_TF_CONS_WT_CUS

This function module makes it possible to transmit a custom object to Monitoring & Alerting, e.g. an invoice. Before transmission, the function module checks the object data for changes and transmits only if changes are found.

The function module has the following import parameters:&#x20;

- IM_CONS_WITH_TRANSPORT_DO (Consignment with transport / the customer-specific object)
- IM_ORG_UNIT (Organizational unit for Monitoring & Alerting)
- IM_DOCUMENT_NUMBER (Document number)

The parameter IM_DOCUMENT_NUMBER has been marked as optional for technical reasons. However, it is required for the proper functioning of the function module and should therefore always be assigned a document number.

The function module has the following export parameters:&#x20;

- EX_RESULT (Report of data transmission)
- EX_HAS_ERROR (Transmission errors occurred)
- EX_IS_TRANSFERRED (Object was transmitted)

The function module has the following exception parameters:

- PARAMETER_EMPTY (Triggered if one of the import parameters is empty)

## Organizational unit for a custom object

BAdI /AEB/MA_CUS_01  - method get_org_unit&#x20;

This method can be used to determine the organizational unit for any business object.

Parameters:

- IM_BUSINESS_OBJECT_NUMBER (Number of business object)
- RE_ORG_UNIT (Organizational unit of target system)

## Transmission of customer specific events

You can send your own custom events from the SAP system to Monitoring & Alerting using the function module '/AEB/MA_PB_TF_TE_CUS'. &#x20;

## Repeat function call for a custom object

BadI /AEB/MA_CUS_02 - create_cwt_do

&#x20;To repeat a function call for a custom business object via the Function Call Monitor, it must be newly determined. The determination must be implemented using this method.    &#x20;

Parameters:

- IM_DATA_SRC_ID_HOST (Unique ID of the business object)
- IM_DOCUMENT_NUMBER (Document number of the business object)
- RE_CWT_DO (Delivery with transports
