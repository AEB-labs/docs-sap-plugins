---
title: Print carrier labels
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
# Read the label byte stream from the response

By setting the document output mode to 'RETURN', the carrier labels are returned as a byte stream. This byte stream can then be sent to a printer from within your SAP system. 

> 📘 Carrier labels are created at package level
>
> In the response data, carrier labels are always returned at **package level**, while other general documents like CMR are provided on header level.

When using methods of class /AEB/CL\_PA\_PB\_CARRIER\_BF, you get the response in the interface structure  /AEB/CL\_PA\_PB\_CR\_SHP\_RES\_DO. As mentioned before, documents can be found in two different areas:

![](https://files.readme.io/c3889518076ca8cf18a86a14ae7955c03aa6c0e139e10efc55cf290e46608c60-image.png)

To read the header documents, e.g. a CMR, use method GET\_DOCUMENTS.

To read the carrier labels, use method GET\_PACKAGE\_RESULTS first. Then loop over the entries of that list. For each entry (each package result) there is a list of documents:

<Image alt="Package results in the response" align="center" width="90% " src="https://files.readme.io/2a9429332ff7a7a54ce9bb97ca3c085f8eb56cd5e3b900d79de2854d4115da9f-image.png">
  Package results in the response
</Image>

In both cases the document data will be returned as binary data in a RAWSTRING format. It will be in the CONTENT field of each document entry. 

In case your using one of the function modules and not the class mentioned above, the documents can be accessed in the export  parameters of the function call.  But the basic principle is the same as explained before, there are documents on header level and others on package level. To print the carrier labels, loop over EX\_PACKAGE\_RESULTS. Docuemnts on header level can be found in the  EX\_DOCUMENTS parameter:

![](https://files.readme.io/982ea52e203bc05e9136331ca7e16ddc8bce4a494ffecc50eb7065692ea26a4b-image.png)

<br />

### Scenario: Trigger carrier labels from the shipping order monitor

Using the "Print labels" - button from the shipping order monitor, data is sent to Carrier Connect. To handle the carrier labels returned in the response,  implement BAdI /AEB/PA\_SHP\_RES\_01. To get the carrier labels, loop over all entries of parameter IM\_PACKAGE\_RESULTS.  For each package result, 1 to n labels are provided in the DOCUMENTS.  

### Scenario: Trigger carrier labels via output message or any other background task

When you trigger the data transfer to Carrier Connect with an output message or any other background task, the response can be handled by implementing BAdI /AEB/PA\_SHP\_RES\_02. To get the carrier labels, loop over all entries of parameter IM\_PACKAGE\_RESULTS.  For each package result, 1 to n labels are provided in the DOCUMENTS.

<br />

# Send the carrier labels to the printer from the SAP system

> 📘 Responsibility for printers, their settings and handling lies is within your IT operation.
>
> AEB just provides the label stream in the appropriate format for the printer. Examples provided here just describe one possible approach, technical availabilities may vary dependend on the infrastructure of your SAP system.

This example outlines the printing when executing an output type for an outbound delivery: 

```Text Print labels executing an output type
FORM print_single_label USING iv_vbeln TYPE vbeln_vl
                            is_doc   TYPE /aeb/pa_pb_dl_doc_do.

  DATA: lv_spool_handle TYPE syst_tabix,
        lv_spool        TYPE rspoid,
        lv_xstring      TYPE xstring,
        lv_filesize     TYPE i,
        lt_raw_buffer   TYPE solix_tab,
        lv_title        TYPE rspotitle,
        lv_name         TYPE rspo0name,
        lv_suffix1      TYPE rspo1name,
        lv_suffix2      TYPE rspo2name,
        lv_ldest        TYPE ldest.

  " Prepare data for output of the labels
  lv_xstring    = is_doc-content.
  lt_raw_buffer = cl_bcs_convert=>xstring_to_solix( iv_xstring = lv_xstring ).
  lv_filesize   = xstrlen( lv_xstring ).

  " Spool-Settings
  CONCATENATE 'AEB-Label' iv_vbeln INTO lv_title SEPARATED BY space.
  lv_name    = 'AEB'.
  lv_suffix2 = iv_vbeln.

  " Determination Printer based on your customer individual logic
  " Printer name equals short name of printer in SPAD
  lv_dest = 'printer_name'.

  " Create Spool Request
  CALL FUNCTION 'RSPO_SR_OPEN'
    EXPORTING
      dest             = lv_ldest
      layout           = 'G_RAW'
      doctype          = 'BIN'
      titleline        = lv_title
      suffix1          = lv_suffix1
      suffix2          = lv_suffix2
      name             = lv_name
      immediate_print  = abap_true       " Direct Print
    IMPORTING
      handle           = lv_spool_handle
      spoolid          = lv_spool
    EXCEPTIONS
      device_missing   = 1
      name_twice       = 2
      no_such_device   = 3
      operation_failed = 4
      OTHERS           = 5.

  IF sy-subrc NE 0.
  " Spool-request could not be created!
  " Create log!!
    RETURN.
  ELSE.
    " Spool-request has been created!
  ENDIF.

  " Add the binary content into Spool Request
  CALL FUNCTION 'RSPO_SR_TABLE_WRITE_BINARY'
    EXPORTING
      handle           = lv_spool_handle
      total            = lv_filesize
    TABLES
      lines            = lt_raw_buffer
    EXCEPTIONS
      handle_not_valid = 1.

  IF sy-subrc NE 0.
  " Create log!!
    RETURN.
  ENDIF.

  " Close Spool Request
  CALL FUNCTION 'RSPO_SR_CLOSE'
    EXPORTING
      handle           = lv_spool_handle
    EXCEPTIONS
      handle_not_valid = 1
      operation_failed = 2
      OTHERS           = 3.

  IF sy-subrc NE 0.
	" Create log!!
    RETURN.
  ENDIF.

ENDFORM.
```
