---
title: Individual address check of business objects
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
To trigger a Compliance check for an existing business object you can use the following function modules:

* /AEB/CMP_CUSTOMER_SCR_CHECK for a customer record
* /AEB/CMP_MD_SCR_CHECK for a material document
* /AEB/CMP_PD_SCR_CHECK for a purchase document
* /AEB/CMP_VENDOR_SCR_CHECK for a vendor record

<br />

<Callout icon="📘" theme="info">
  These functions only carry out a simple screening check and return just a boolean ( true / false) whether the business object is rated as critical (match). However, this does not set any blocks in the SAP business object and does not create an entry in the Compliance Monitor.
</Callout>
