---
title: Change determined profile ID
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The AEB Connector collects some the data from the business object depending on a collector profile. This profile is assigned to the export interface in the customizing. If you have special logic on which profile to use, you can use the BAdIs provided for each business object. The BAdI has the Method CHG\_PROFILE\_ID\_FOR\_\<BUSINESS\_OBJECT> with the header data of the according business object and a object containing the profile ID as parameters.

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
  </tbody>
</Table>
