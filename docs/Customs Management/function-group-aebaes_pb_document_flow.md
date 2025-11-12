---
title: Document Flow
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
To include customs declarations and customs documents in the document flow of the supported transactions, we provide the function group /AEB/AES_PB_DOCUMENT_FLOW
[block:api-header]
{
  "title": "Function module /AEB/AES_PB_ADD_SUBSEQUENT_DOC"
}
[/block]
This function module extends the document flow overview of the SAP standard by the customs declarations and documents generated with the Customs Management Plug-in for SAP®.
[block:code]
{
  "codes": [
    {
      "code": "  CALL FUNCTION '/AEB/AES_PB_ADD_SUBSEQUENT_DOC'\n    EXPORTING\n      im_beleg    = f_beleg\n      im_vbtyp    = f_vbtyp\n    CHANGING\n      ch_vbfa_tab = f_vbfa_tab.",
      "language": "text",
      "name": "Example call in SAP BAdI BADI_SD_DOCUMENTFLOW Method ADD_SUBSEQUENT_DOCUMENT"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Function module /AEB/AES_PB_SET_STATUS"
}
[/block]
This function module determines the status of the customs declarations and their documents that are displayed in the SAP® standard document flow.
[block:code]
{
  "codes": [
    {
      "code": "  CALL FUNCTION '/AEB/AES_PB_SET_STATUS'\n    EXPORTING\n      im_vbfa       = f_vbfa\n    CHANGING\n      ch_tvbst      = f_tvbst\n      ch_status_set = f_status_set.",
      "language": "text",
      "name": "Example call in SAP BAdI BADI_SD_DOCUMENTFLOW Method SET_STATUS"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Function module /AEB/AES_PB_DISPLAY_DOCUMENT"
}
[/block]
You can use this function module to edit customs declarations and documents from the document flow. The status of the document displayed is set to Printed. This function module only functions in context or interaction with function module /AEB/AES_PB_ADD_SUBSEQUENT_DOC and therefore within the document flow.
[block:code]
{
  "codes": [
    {
      "code": "  CALL FUNCTION '/AEB/AES_PB_DISPLAY_DOCUMENT'\n    EXPORTING\n      im_beleg = f_beleg.",
      "language": "text",
      "name": "Example call in SAP BAdI BADI_SD_DOCUMENTFLOW Method DISPLAY_DOCUMENT"
    }
  ]
}
[/block]