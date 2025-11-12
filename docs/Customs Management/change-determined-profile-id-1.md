---
title: Change determined profile ID
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
The Customs Management Plug-In collects some the data from the business object depending on a collector profile. This profile is assigned to the export interface in the customizing. If you have special logic on which profile to use, you can use the BAdIs provided for each business object. The BAdI has the Method CHG\_PROFILE\_ID\_FOR\_<BUSINESS_OBJECT> with the header data of the according business object and an object containing the profile ID as parameters.
[block:parameters]
{
  "data": {
    "h-0": "BAdI",
    "h-1": "Business Object",
    "0-0": "/AEB/80_PROF_SDV_01",
    "0-1": "Delivery",
    "1-0": "/AEB/80_PROF_INV_01",
    "2-0": "/AEB/80_PROF_SHP_01",
    "1-1": "Invoice",
    "2-1": "Shipment",
    "3-0": "/AEB/80_PROF_PD_01",
    "4-0": "/AEB/80_PROF_II_01",
    "3-1": "Purchase document",
    "4-1": "Incoming Invoice",
    "5-0": "/AE1/80_PROF_FRO_01",
    "5-1": "Freight order",
    "6-0": "/AEB/80_PROF_MD_01",
    "6-1": "Material document"
  },
  "cols": 2,
  "rows": 7
}
[/block]