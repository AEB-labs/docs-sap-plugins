---
title: Basic concept of transfer material
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: >-
    Like to learn more about basic concepts? Or start with adapting the transfer
    of materials?
  pages:
    - type: basic
      slug: basic-concept-of-read-material
      title: Basic concept of read material
    - type: basic
      slug: examples-for-badi-implementations
      title: Adapt the transfer of materials
---
The transfer of a material is splitted several parts. The following diagram shows those parts.

![871](https://files.readme.io/cf94c92-2023-02-27_164805.jpg "2023-02-27_164805.jpg")

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Things to do
      </th>

      <th style={{ textAlign: "left" }}>
        Example
      </th>

      <th style={{ textAlign: "left" }}>
        Configurable?
      </th>

      <th style={{ textAlign: "left" }}>
        Custom Code
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Determine organizational unit
      </td>

      <td style={{ textAlign: "left" }}>
        Can be assigned by plant or by sales org or just by client.
      </td>

      <td style={{ textAlign: "left" }}>
        Yes
      </td>

      <td style={{ textAlign: "left" }}>
        No
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Determine engine client
      </td>

      <td style={{ textAlign: "left" }}>
        The engine client is assigned to the organizational unit, but it is possible to assign the same engine client to multiple organizational units.
      </td>

      <td style={{ textAlign: "left" }}>
        Yes
      </td>

      <td style={{ textAlign: "left" }}>
        No
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Determine product group
      </td>

      <td style={{ textAlign: "left" }}>
        Product group can be assigned by material type and material category.
      </td>

      <td style={{ textAlign: "left" }}>
        Yes
      </td>

      <td style={{ textAlign: "left" }}>
        Yes
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Collect material data
      </td>

      <td style={{ textAlign: "left" }}>
        Provides all collected data which is to be transfered for the  material.
      </td>

      <td style={{ textAlign: "left" }}>
        Yes
      </td>

      <td style={{ textAlign: "left" }}>
        Yes
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Check classification values
      </td>

      <td style={{ textAlign: "left" }}>
        The plug-in sends only  classification values again, if there were changes since the last update. Otherwise existing changes in Product Classification could be overwritten.
      </td>

      <td style={{ textAlign: "left" }}>
        Yes, /AEB/TA\_WORK\_IN\_PROCESS\_INIT has a flag where you disable/enable this behavior. In context of saving materials it is enabled by default. You cannot change this.
      </td>

      <td style={{ textAlign: "left" }}>
        No
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Check is material to be deleted
      </td>

      <td style={{ textAlign: "left" }}>
        The material exists in SAP, but should not exist in Product Classification.
      </td>

      <td style={{ textAlign: "left" }}>
        No
      </td>

      <td style={{ textAlign: "left" }}>
        No
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Transfer material data
      </td>

      <td style={{ textAlign: "left" }}>
        The web service is called and the result will be handled, logs written etc.
      </td>

      <td style={{ textAlign: "left" }}>
        No
      </td>

      <td style={{ textAlign: "left" }}>
        No
      </td>
    </tr>
  </tbody>
</Table>
