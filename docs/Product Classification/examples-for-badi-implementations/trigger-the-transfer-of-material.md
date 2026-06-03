---
title: Trigger the transfer of a material record
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
  pages:
    - type: basic
      slug: adapt-material-data-to-transfer
      title: Adapt material data to transfer
---
In some cases it might be necessary to trigger the update of the material via API. For this, use the function module /AEB/TA_PB_UPDATE_MATERIAL which does exactly the same, as if the update is triggered by saving the material via transaction MM02.  Call the function by passing the material number.
