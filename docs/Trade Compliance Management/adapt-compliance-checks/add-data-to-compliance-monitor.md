---
title: Additional columns in the Compliance Monitor
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
There are BAdIs for each business object to extend data in the Compliance Monitor.  
You can put you're own z-fields to the Compliance Monitor entries and fill this field with the following BAdIs or you just can overwrite the standard filling of the existing fields. The BAdI will be called when the monitor entry is saved and not when opening the Compliance Monitor or reading entries from the Compliance Monitor.  

[block:parameters]
{
  "data": {
    "h-0": "Business Object",
    "h-1": "BAdI",
    "0-0": "Account document",
    "0-1": "/AEB/CMP_AD_MON_01",
    "1-0": "Applicant",
    "1-1": "/AEB/CMP_AP_MON_01",
    "2-0": "Bank",
    "2-1": "/AEB/CMP_BNK_MON_01",
    "3-0": "Business partner",
    "3-1": "/AEB/CMP_BP_MON_01",
    "4-0": "Customer",
    "4-1": "/AEB/CMP_CUS_MON_01",
    "5-0": "Delivery",
    "5-1": "/AEB/CMP_DLV_MON_01  \n/AEB/CMP_EC_DLV_02",
    "6-0": "Employee",
    "6-1": "/AEB/CMP_EM_MON_01",
    "7-0": "Material document",
    "7-1": "/AEB/CMP_MD_MON_01",
    "8-0": "Payment",
    "8-1": "/AEB/CMP_PIP_MON_01",
    "9-0": "Purchase document",
    "9-1": "/AEB/CMP_PD_MON_01",
    "10-0": "Sales document",
    "10-1": "/AEB/CMP_SDOC_MON_01  \n/AEB/CMP_EC_ORDER_05",
    "11-0": "Service notification",
    "11-1": "/AEB/CMP_SN_MON_01",
    "12-0": "Service order",
    "12-1": "/AEB/CMP_SO_MON_01",
    "13-0": "Vendor",
    "13-1": "/AEB/CMP_VDR_MON_01"
  },
  "cols": 2,
  "rows": 14,
  "align": [
    "left",
    "left"
  ]
}
[/block]


## Expand Compliance Structures

You can expand the following structures with the help of an APPEND. The names of the new fields have to start with ZZ.

| Application                           | Structure                    |
| :------------------------------------ | :--------------------------- |
| EC-Cockpit - Overview of item results | /AEB/CMP_PB_CUS_EC_ITM_DO    |
| Compliance Monitor - Overview         | /AEB/CMP_PB_CUS_MON_ENTRY_DO |