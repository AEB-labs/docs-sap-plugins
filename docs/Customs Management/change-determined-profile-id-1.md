---
title: Change determined profile ID
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
The Customs Management Plug-In collects some the data from the business object depending on a collector profile. This profile is assigned to the export interface in the customizing. If you have special logic on which profile to use, you can use the BAdIs provided for each business object. The BAdI has the Method CHG\_PROFILE\_ID\_FOR\_\<BUSINESS\_OBJECT> with the header data of the according business object and an object containing the profile ID as parameters.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        BAdI
      </th>

      <th style={{ textAlign: "left" }}>
        Business Object
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/80\_PROF\_SDV\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Delivery
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/80\_PROF\_INV\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Invoice
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/80\_PROF\_SHP\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Shipment
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/80\_PROF\_PD\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Purchase document
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/80\_PROF\_II\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Incoming Invoice
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AE1/80\_PROF\_FRO\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Freight order
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/80\_PROF\_MD\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Material document
      </td>
    </tr>
  </tbody>
</Table>
