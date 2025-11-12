---
title: Change determined profile ID
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The add-on collects the data from the business object depending on a collector profile. This profile is assigned to the export interface in the configuration. If you have special logic for selecting a different profile, you can use the BAdIs provided for each business object. The BAdI has the Method CHG\_PROFILE\_ID\_FOR\_\<BUSINESS_OBJECT> with the header data of the according business object and an object containing the profile ID as parameters.

| BAdI                | Business Object |
| :------------------ | :-------------- |
| /AE1/80_PROF_FRO_01 | Freight order   |