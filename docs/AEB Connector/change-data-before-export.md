---
title: Change Data before export
excerpt: After standard filling BAdIs
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: set-document-currency
      title: Set document currency
    - type: basic
      slug: replace-invoice-number-with-delivery-number
      title: Replace Invoice number with delivery number
    - type: basic
      slug: add-additional-fields
      title: Add additional Fields
---
Every exported business object has two BAdIs which are executed depending on the chosen target interface. The BAdI contains the method 'AFTER_STD_FILLING' which is executed after the data for the target interface is collected and before the data is sent. Each BAdI contains its corresponding data object which is to be changed, the header data of the corresponding business object and two factories. The IM_NULLABLE_VALUE_FACOTRY can be used to generate so called Nullable Values (see [Determination and allocation of value](doc:determination-and-allocation-of-value) ).  The IM_AS4CO_DATA_OBJECT_FACTORY can be used to generate objects used in the interface.
[block:parameters]
{
  "data": {
    "h-0": "BAdI Name",
    "h-1": "Business Object",
    "h-2": "Target Interface",
    "0-0": "/AEB/CT_EXP_SDV_01",
    "0-1": "Delivery",
    "0-2": "Export consignment",
    "1-0": "/AEB/CT_EXP_SDV_02",
    "1-1": "Delivery",
    "1-2": "Delivery note",
    "2-0": "/AEB/CT_EXP_INV_01",
    "2-1": "Invoice",
    "2-2": "Export consignment",
    "3-0": "/AEB/CT_EXP_INV_02",
    "3-1": "Invoice",
    "3-2": "Delivery note",
    "4-0": "/AEB/CT_EXP_SHP_01",
    "5-0": "/AEB/CT_EXP_SHP_02",
    "4-1": "Shipment",
    "5-1": "Shipment",
    "4-2": "Export consignment",
    "5-2": "Delivery note"
  },
  "cols": 3,
  "rows": 6
}
[/block]