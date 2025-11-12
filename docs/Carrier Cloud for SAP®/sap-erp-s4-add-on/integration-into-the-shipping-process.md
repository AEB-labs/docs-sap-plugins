---
title: Integration into the shipping process
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
Logistics processes in shipping are diverse. And SAP systems offer many variants for dispatch processing. Regardless of this, the AEB functions can be used flexibly to trigger data transfers. In general, this trigger is the completion of a certain packing step (all packages or a single package).

[block:parameters]
{
  "data": {
    "h-0": "Trigger",
    "h-1": "AEB functionality",
    "0-0": "At a **packing station**, the user triggers an action like \"packing completed\"-button. The packing station is a  customer specific development or provided by a 3rd party",
    "0-1": "From the customized packing station, use the [class](https://sap-plugins.docs.developers.aeb.com/docs/create-shipment)  or [function module](https://sap-plugins.docs.developers.aeb.com/docs/shipment-processing) of the add-on to create or update the shipment.",
    "1-0": "The user triggers the \"print label\"-button at a the **packing station** provided from SAP standard ( HUPAST )",
    "1-1": "From BAdI BADI_HU_PRINT, use the class or function module.",
    "2-0": "An **outbound delivery** gets saved with new packing data and you want to print a label for the new package",
    "2-1": "Create an Z-program that uses the class / function module and trigger that program from an output message.  \nOr use the class / function module from a suitable user exit like MV50AFZ1 , SAVE_DOCUMENT",
    "3-0": "An **outbound delivery **is completely packed and you want to print the labels for the all the packages in the delivery at once",
    "3-1": "Define an output message with the AEB standard program.  \nOr create an Z-program that calls the class / function module and trigger this program from an output message.  \nOr execute the class / function module from a suitable user exit.",
    "4-0": "User sets one of the status in a **shipment **(LE-TRA) ",
    "4-1": "Use enhancement V56FSTAT, customer exit  EXIT_SAPLV56F_012 to execute the class / function module",
    "5-0": "A **freight order** gets saved with new packing data and you want to print a label for the new package",
    "5-1": "Create an Z-program that uses the class / function module and trigger a PPF action to execute that program",
    "6-0": "A **freight order** has been packed completely and you want to print the labels for the all the packages at once",
    "6-1": "Trigger PPF action to execute the AEB standard program "
  },
  "cols": 2,
  "rows": 7,
  "align": [
    "left",
    "left"
  ]
}
[/block]