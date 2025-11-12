---
title: Additional columns in the Compliance Monitor
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
There are BAdIs for each business object to extend data in the Compliance Monitor.\
You can put you're own z-fields to the Compliance Monitor entries and fill this field with the following BAdIs or you just can overwrite the standard filling of the existing fields. The BAdI will be called when the monitor entry is saved and not when opening the Compliance Monitor or reading entries from the Compliance Monitor.  

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Business Object
      </th>

      <th style={{ textAlign: "left" }}>
        BAdI
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Account document
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_AD\_MON\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Applicant
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_AP\_MON\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Bank
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_BNK\_MON\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Business partner
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_BP\_MON\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Customer
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_CUS\_MON\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Delivery
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_DLV\_MON\_01\
        /AEB/CMP\_EC\_DLV\_02
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Employee
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_EM\_MON\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Material document
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_MD\_MON\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Payment
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_PIP\_MON\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Purchase document
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_PD\_MON\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Sales document
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_SDOC\_MON\_01\
        /AEB/CMP\_EC\_ORDER\_05
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Service notification
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_SN\_MON\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Service order
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_SO\_MON\_01
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Vendor
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/CMP\_VDR\_MON\_01
      </td>
    </tr>
  </tbody>
</Table>

## Expand Compliance Structures

You can expand the following structures with the help of an APPEND. The names of the new fields have to start with ZZ.

| Application                           | Structure                         |
| :------------------------------------ | :-------------------------------- |
| EC-Cockpit - Overview of item results | /AEB/CMP\_PB\_CUS\_EC\_ITM\_DO    |
| Compliance Monitor - Overview         | /AEB/CMP\_PB\_CUS\_MON\_ENTRY\_DO |
