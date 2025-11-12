---
title: Document context functions
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
This section lists the functions that can be used in a context of a concrete document. Choose the according object class for instantiation based on the document type:

| Document type     | Class to use                    |
| :---------------- | :------------------------------ |
| Billing document  | /AEB/CL\_AES\_PB\_INV\_DECL\_BC |
| Purchase document | /AEB/CL\_AES\_PB\_PD\_DECL\_BC  |
| Delivery document | /AEB/CL\_AES\_PB\_SDV\_DECL\_BC |
| Shipment document | /AEB/CL\_AES\_PB\_SHP\_DECL\_BC |
| Incoming invoice  | /AEB/CL\_AES\_PB\_II\_DECL\_BC  |
| Freight order     | /AE1/CL\_AES\_PB\_FRO\_DECL\_BC |
| Material document | /AEB/CL\_AES\_PB\_MD\_DECL\_BC  |

## Querying customs declarations data

You can use the method  *get\_declarations* to query all associated customs declarations for a document that have already been synchronized.

```text Example coding to query ICI customs declarations for a invoice document and read their MRN
DATA:
  inv_decl_bc           TYPE REF TO /aeb/cl_aes_pb_inv_decl_bc,
  exception             TYPE REF TO /aeb/cx_aes_pb_get_decl_sc,
  result_do             TYPE REF TO /aeb/cl_aes_pb_get_decl_res_do,
  declaration_dos       TYPE /aeb/aes_if_aes_pb_decl_dos,
  declaration_do        TYPE REF TO /aeb/if_aes_pb_declaration_do,
  error_text            TYPE string,
  document_no           TYPE /aeb/aes_pb_doc_no VALUE '90017468',
  mrn_nv                TYPE REF TO /aeb/cl_01_char_35_nv,
  mrn                   TYPE string.

inv_decl_bc = /aeb/cl_aes_pb_inv_decl_bc=>new_for(
	im_doc_no = document_no ).

TRY.
    result_do = inv_decl_bc->get_declarations( ).
    declaration_dos = result_do->get_ici_declarations( ).
    LOOP AT declaration_dos INTO declaration_do.
      mrn_nv = declaration_do->get_customs_registration_numbe( ).
      mrn = mrn_nv->v.
*     do something with MRN
    ENDLOOP.
  CATCH /aeb/cx_aes_pb_get_decl_sc INTO exception.
    error_text = exception->if_message~get_text( ).
*   react to errors
ENDTRY.
```

## Marking documents as printed

When handling customs documents, it can be rqeuired that you want to print them in an individual way which the standard solution doesn't support. In this case you want to tell the standard application that you have printed the document yourself or rather mark the document as printed. To do this, use the class /AEB/CL\_AES\_PB\_DOC\_STA\_BC with the method MARK\_DOCUMENT\_AS\_PRINTED.
