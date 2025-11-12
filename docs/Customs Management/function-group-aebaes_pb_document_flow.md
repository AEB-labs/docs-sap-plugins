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
To include customs declarations and customs documents in the document flow of the supported transactions, we provide the function group /AEB/AES\_PB\_DOCUMENT\_FLOW

## Function module /AEB/AES\_PB\_ADD\_SUBSEQUENT\_DOC

This function module extends the document flow overview of the SAP standard by the customs declarations and documents generated with the Customs Management Plug-in for SAP®.

```text Example call in SAP BAdI BADI_SD_DOCUMENTFLOW Method ADD_SUBSEQUENT_DOCUMENT
CALL FUNCTION '/AEB/AES_PB_ADD_SUBSEQUENT_DOC'
    EXPORTING
      im_beleg    = f_beleg
      im_vbtyp    = f_vbtyp
    CHANGING
      ch_vbfa_tab = f_vbfa_tab.
```

## Function module /AEB/AES\_PB\_SET\_STATUS

This function module determines the status of the customs declarations and their documents that are displayed in the SAP® standard document flow.

```text Example call in SAP BAdI BADI_SD_DOCUMENTFLOW Method SET_STATUS
CALL FUNCTION '/AEB/AES_PB_SET_STATUS'
    EXPORTING
      im_vbfa       = f_vbfa
    CHANGING
      ch_tvbst      = f_tvbst
      ch_status_set = f_status_set.
```

## Function module /AEB/AES\_PB\_DISPLAY\_DOCUMENT

You can use this function module to edit customs declarations and documents from the document flow. The status of the document displayed is set to Printed. This function module only functions in context or interaction with function module /AEB/AES\_PB\_ADD\_SUBSEQUENT\_DOC and therefore within the document flow.

```text Example call in SAP BAdI BADI_SD_DOCUMENTFLOW Method DISPLAY_DOCUMENT
CALL FUNCTION '/AEB/AES_PB_DISPLAY_DOCUMENT'
    EXPORTING
      im_beleg = f_beleg.
```
