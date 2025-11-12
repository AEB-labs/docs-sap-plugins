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

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Document type
      </th>

      <th style={{ textAlign: "left" }}>
        BAdI
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Delivery
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/AES\_AR\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Incoming invoice
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/AES\_AR\_02
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Invoice
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/AES\_AR\_03
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Purchase document
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/AES\_AR\_04
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Shipment
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/AES\_AR\_05
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Freight order\
         (available with version 1.1)
      </td>

      <td style={{ textAlign: "left" }}>
        /AE1/AES\_AR\_06
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Material document\
         (available with version 1.1)
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/AES\_AR\_06
      </td>
    </tr>
  </tbody>
</Table>

## Notes

Every BAdI definition contains a factory object that you can use to create new data objects.

You have to use predefined constants to refer to the SAP object and the archive type. The constant values are available in the interfaces

* /aeb/if\_01\_pb\_ar\_obj\_do for the SAP objects
* /aeb/if\_aes\_pb\_ar\_type\_obj\_do for the archive types

There are currently two archive types implemented:

* **Archive storage**: This type is relevant for objects of type delivery, invoice, incoming invoice, purchase document and shipment. Documents are saved in your ArchiveLink repository and linked to the business object.
* **Attachment**: This type is only relevant for objects of type freight order. Documents are added to the attachment folder of the freight order.

## Example implementations

```text Add customs document to an invoice
DATA:
    ar_obj_no      TYPE /aeb/if_01_pb_ar_obj_do=>t_ar_obj_no,
    ar_obj_nos     TYPE /aeb/if_01_pb_ar_obj_do=>tt_ar_obj_no,
    ar_obj_do      TYPE REF TO /aeb/if_01_pb_ar_obj_do,
    ar_obj_dos     TYPE /aeb/if_01_pb_ar_obj_do=>tt_ar_obj_do,
    ar_type_obj_do TYPE REF TO /aeb/if_aes_pb_ar_type_obj_do.

  ar_obj_no = '0090000001'. "Replace this line with the determination of the invoice number.
  APPEND ar_obj_no TO ar_obj_nos.

  ar_obj_do = im_ar_data_object_factory->new_01_pb_ar_obj_do(
                im_sap_object        = /aeb/if_01_pb_ar_obj_do=>c_sap_object_invoice
                im_ar_object_numbers = ar_obj_nos ).
  APPEND ar_obj_do TO ar_obj_dos.

  ar_type_obj_do = im_ar_data_object_factory->new_aes_pb_ar_type_obj_do(
                     im_archive_type = /aeb/if_aes_pb_ar_type_obj_do=>c_archive_type_arch_storage
                     im_ar_obj_dos   = ar_obj_dos ).
  APPEND ar_type_obj_do TO ch_value.
```
