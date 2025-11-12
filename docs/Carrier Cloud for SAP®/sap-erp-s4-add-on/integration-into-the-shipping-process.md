---
title: Integration into the shipping process
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
Logistics processes in shipping are diverse. And SAP systems offer many variants for dispatch processing. Regardless of this, the AEB functions can be used flexibly to trigger data transfers. In general, this trigger is the completion of a certain packing step (all packages or a single package).

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Trigger
      </th>

      <th style={{ textAlign: "left" }}>
        AEB functionality
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        At a **packing station**, the user triggers an action like "packing completed"-button. The packing station is a  customer specific development or provided by a 3rd party
      </td>

      <td style={{ textAlign: "left" }}>
        From the customized packing station, use the [class](https://sap-plugins.docs.developers.aeb.com/docs/create-shipment)  or [function module](https://sap-plugins.docs.developers.aeb.com/docs/shipment-processing) of the add-on to create or update the shipment.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        The user triggers the "print label"-button at a the **packing station** provided from SAP standard ( HUPAST )
      </td>

      <td style={{ textAlign: "left" }}>
        From BAdI BADI\_HU\_PRINT, use the class or function module.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        An **outbound delivery** gets saved with new packing data and you want to print a label for the new package
      </td>

      <td style={{ textAlign: "left" }}>
        Create an Z-program that uses the class / function module and trigger that program from an output message.\
        Or use the class / function module from a suitable user exit like MV50AFZ1 , SAVE\_DOCUMENT
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        An **outbound delivery**is completely packed and you want to print the labels for the all the packages in the delivery at once
      </td>

      <td style={{ textAlign: "left" }}>
        Define an output message with the AEB standard program.\
        Or create an Z-program that calls the class / function module and trigger this program from an output message.\
        Or execute the class / function module from a suitable user exit.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        User sets one of the status in a **shipment**(LE-TRA) 
      </td>

      <td style={{ textAlign: "left" }}>
        Use enhancement V56FSTAT, customer exit  EXIT\_SAPLV56F\_012 to execute the class / function module
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        A **freight order** gets saved with new packing data and you want to print a label for the new package
      </td>

      <td style={{ textAlign: "left" }}>
        Create an Z-program that uses the class / function module and trigger a PPF action to execute that program
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        A **freight order** has been packed completely and you want to print the labels for the all the packages at once
      </td>

      <td style={{ textAlign: "left" }}>
        Trigger PPF action to execute the AEB standard program 
      </td>
    </tr>
  </tbody>
</Table>
