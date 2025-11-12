---
title: BAdIs for archiving
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
These BAdIs enable you to determine further SAP objects for which your customs documents (e.g. ABD and AGV for German Export) should be archived and linked to. 
[block:parameters]
{
  "data": {
    "0-0": "Delivery",
    "h-0": "Document type",
    "h-1": "BAdI",
    "1-0": "Incoming invoice",
    "2-0": "Invoice",
    "3-0": "Purchase document",
    "4-0": "Shipment",
    "0-1": "/AEB/AES_AR_01",
    "1-1": "/AEB/AES_AR_02",
    "2-1": "/AEB/AES_AR_03",
    "3-1": "/AEB/AES_AR_04",
    "4-1": "/AEB/AES_AR_05",
    "5-0": "Freight order\n (available with version 1.1)",
    "5-1": "/AE1/AES_AR_06",
    "6-0": "Material document\n (available with version 1.1)",
    "6-1": "/AEB/AES_AR_06"
  },
  "cols": 2,
  "rows": 7
}
[/block]

[block:api-header]
{
  "title": "Notes"
}
[/block]
Every BAdI definition contains a factory object that you can use to create new data objects.

You have to use predefined constants to refer to the SAP object and the archive type. The constant values are available in the interfaces
* /aeb/if_01_pb_ar_obj_do for the SAP objects
* /aeb/if_aes_pb_ar_type_obj_do for the archive types

There are currently two archive types implemented:
* **Archive storage**: This type is relevant for objects of type delivery, invoice, incoming invoice, purchase document and shipment. Documents are saved in your ArchiveLink repository and linked to the business object.
* **Attachment**: This type is only relevant for objects of type freight order. Documents are added to the attachment folder of the freight order.
[block:api-header]
{
  "title": "Example implementations"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "  DATA:\n    ar_obj_no      TYPE /aeb/if_01_pb_ar_obj_do=>t_ar_obj_no,\n    ar_obj_nos     TYPE /aeb/if_01_pb_ar_obj_do=>tt_ar_obj_no,\n    ar_obj_do      TYPE REF TO /aeb/if_01_pb_ar_obj_do,\n    ar_obj_dos     TYPE /aeb/if_01_pb_ar_obj_do=>tt_ar_obj_do,\n    ar_type_obj_do TYPE REF TO /aeb/if_aes_pb_ar_type_obj_do.\n\n  ar_obj_no = '0090000001'. \"Replace this line with the determination of the invoice number.\n  APPEND ar_obj_no TO ar_obj_nos.\n\n  ar_obj_do = im_ar_data_object_factory->new_01_pb_ar_obj_do(\n                im_sap_object        = /aeb/if_01_pb_ar_obj_do=>c_sap_object_invoice\n                im_ar_object_numbers = ar_obj_nos ).\n  APPEND ar_obj_do TO ar_obj_dos.\n\n  ar_type_obj_do = im_ar_data_object_factory->new_aes_pb_ar_type_obj_do(\n                     im_archive_type = /aeb/if_aes_pb_ar_type_obj_do=>c_archive_type_arch_storage\n                     im_ar_obj_dos   = ar_obj_dos ).\n  APPEND ar_type_obj_do TO ch_value.\n",
      "language": "text",
      "name": "Add customs document to an invoice"
    }
  ]
}
[/block]