---
title: Custom objects
deprecated: false
hidden: false
metadata:
  robots: index
---
The add-on provides functionality for outbound deliveries, shipments, freight orders and sales orders. But apart from that you can also transfer any other documents from your SAP system. To support this, the add-on provides functions for custom object or "any objects" as follows.&#x20;

## Organizational unit for a custom object

BAdI /AEB/MA_CUS_01  - method get_org_unit&#x20;

This method can be used to determine the organizational unit for any business object.

Parameters:

- IM_BUSINESS_OBJECT_NUMBER (Number of business object)
- RE_ORG_UNIT (Organizational unit of target system)

<br />

## Repeat function call for a custom object

BadI /AEB/MA_CUS_02 - create_cwt_do

&#x20;To repeat a function call for a custom business object via the Function Call Monitor, it must be newly determined. The determination must be implemented using this method.    &#x20;

Parameters:

- IM_DATA_SRC_ID_HOST (Unique ID of the business object)
- IM_DOCUMENT_NUMBER (Document number of the business object)
- RE_CWT_DO (Delivery with transports
