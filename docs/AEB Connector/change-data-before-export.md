---
title: Change Data before export
excerpt: After standard filling BAdIs
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: set-document-currency
      title: Set document currency
    - type: basic
      slug: replace-invoice-number-with-delivery-number
      title: Replace Invoice number with delivery number
    - type: basic
      slug: add-additional-fields
      title: Add additional Fields
---
Every exported business object has two BAdIs which are executed depending on the chosen target interface. The BAdI contains the method 'AFTER\_STD\_FILLING' which is executed after the data for the target interface is collected and before the data is sent. Each BAdI contains its corresponding data object which is to be changed, the header data of the corresponding business object and two factories. The IM\_NULLABLE\_VALUE\_FACOTRY can be used to generate so called Nullable Values (see [Determination and allocation of value](doc:determination-and-allocation-of-value) ).  The IM\_AS4CO\_DATA\_OBJECT\_FACTORY can be used to generate objects used in the interface.

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        BAdI Name
      </th>

      <th style={{ textAlign: "left" }}>
        Business Object
      </th>

      <th style={{ textAlign: "left" }}>
        Target Interface
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/CT\_EXP\_SDV\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Delivery
      </td>

      <td style={{ textAlign: "left" }}>
        Export consignment
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/CT\_EXP\_SDV\_02
      </td>

      <td style={{ textAlign: "left" }}>
        Delivery
      </td>

      <td style={{ textAlign: "left" }}>
        Delivery note
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/CT\_EXP\_INV\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Invoice
      </td>

      <td style={{ textAlign: "left" }}>
        Export consignment
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/CT\_EXP\_INV\_02
      </td>

      <td style={{ textAlign: "left" }}>
        Invoice
      </td>

      <td style={{ textAlign: "left" }}>
        Delivery note
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/CT\_EXP\_SHP\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Shipment
      </td>

      <td style={{ textAlign: "left" }}>
        Export consignment
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/CT\_EXP\_SHP\_02
      </td>

      <td style={{ textAlign: "left" }}>
        Shipment
      </td>

      <td style={{ textAlign: "left" }}>
        Delivery note
      </td>
    </tr>
  </tbody>
</Table>
