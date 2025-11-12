---
title: Individual address check
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
The following functions are available to trigger the screening check in a flexible way. 

> 📘 Please note that these functions will not create any entries in the Compliance monitor entries nor logs .        

If you would like to do a screening check for any kind of address (even if there is not an existing document / master data record in the system)  you can use one of the following function modules:  
(1) /AEB/CMP_ADDR_CHK_FOR_ADDR1VAL  
(2) /AEB/CMP_ADDR_CHK_FOR_ADDRNR  
(3) /AEB/CMP_ADDR_CHK_FOR_HANDLE

Each function module provides the following import parameters:

[block:parameters]
{
  "data": {
    "h-0": "Field name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "(1) IM_ADDR1_VAL  \n(2) IM_ADDRNR  \n(3) IM_ADDRHANDLE",
    "0-1": "(1) ADDR1_VAL  \n(2) ADDR1_VAL-ADDRNUMBER  \n(3) ADDR1_VAL-ADDRHANDLE",
    "0-2": "(1) Structure with address values  \n(2) address number  \n(3) address handle",
    "1-0": "IM_SUPPRESS_ENGINE_LOGGING",
    "1-1": "CHAR1",
    "1-2": "Flag to supress logs",
    "2-0": "IM_RFC_DESTINATION",
    "2-1": "RFCDEST",
    "2-2": "Defined destination to AEB engine",
    "3-0": "IM_ENGINE_CLIENT",
    "3-1": "/AEB/CMP_ENGN_CLIENT",
    "3-2": "Client name from the AEB engine",
    "4-0": "IM_ENGINE_PROFILE",
    "4-1": "/AEB/CMP_ENGN_PROFILE",
    "4-2": "Profile Name from the AEB engine",
    "5-0": "IM_REF_COMMENT",
    "5-1": "CHAR255",
    "5-2": "Comment for the check – e.g. a document ID or master data ID  (optional)",
    "6-0": "IM_REF_ID",
    "6-1": "/AEB/CMP_ENTRY_REF_ID",
    "6-2": "An internal reference number (optional)",
    "7-0": "IM_ORG_UNIT",
    "7-1": "/AEB/CMP_PB_ORG_UNIT",
    "7-2": "If provided, used to retrieve engine client and profile. Replaces the corresponding import parameters.",
    "8-0": "IM_ADDRESS_TYPE",
    "8-1": "CHAR20",
    "8-2": "Possible values are:  \n•\t\"individual\"  \n•\t\"entity\"  \n•\t\"meansOfTransport\"  \n•\t\"unknown\""
  },
  "cols": 3,
  "rows": 9,
  "align": [
    "left",
    "left",
    "left"
  ]
}
[/block]