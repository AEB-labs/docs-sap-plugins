---
title: Simple shipping
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
The AEB add-on provides predetermined function modules to support the creation and processing of  a shipping order in Carrier Cloud.  The term "predetermined" means that the function modules are usable for certain shipping scenarios but have some limitations and are less flexible. In example, the workstation or the mode of printing cannot be changed and will be detremined based on the configuration.  Another example for such limitations are special processes like document uploads or paperless trade.

<Callout icon="📘" theme="info">
  #### The functions emntioned in this section are for certain pre-determined scenarios only. To make use of the functionality with more flexibility, use the classes and methods documented in [Advanced Shipping](https://sap-plugins.docs.developers.aeb.com/docs/processing-more-in-detail).
</Callout>

# Supported business objects

Using the provided functions modules, you can transfer the following SAP business objects to Carrier Cloud for creating or updating a shipping order there:

* Outbound delivery
* Shipment (LE-TRA)
* TM business objects: freight order, freight booking and consignment

# Pre-determined shipping scenarios

## Outbound deliveries and shipments

| Combined tasks                                                                                         | FM for outbound deliveries     | FM for shipments               |
| :----------------------------------------------------------------------------------------------------- | :----------------------------- | :----------------------------- |
| Create the shipping order <br /> Prepare all labels <br /> Complete the shipping order <br />          | /AEB/PA_PB_DLV_CR_SHP_W_COMP   | /AEB/PA_PB_SHP_CR_SHP_W_COMP   |
| Create the shipping order <br /> Prepare and print all labels <br /> Complete the shipping order       | /AEB/PA_PB_DLV_CR_SHP_W_PRINT  | /AEB/PA_PB_SHP_CR_SHP_W_PRINT  |
| Create the shipping order <br /> No preparation, no print <br /> No completion                         | /AEB/PA_PB_DLV_CR_SHP          | /AEB/PA_PB_SHP_CR_SHP          |
| Prepare and print labels <br /> Complete the shipping order                                            | /AEB/PA_PB_DLV_PRINT_SHP       | /AEB/PA_PB_SHP_PRINT_SHP       |
| Prepare labels <br /> Complete the shipping order                                                      | /AEB/PA_PB_DLV_COMPLETE_SHP    | /AEB/PA_PB_SHP_COMPLETE_SHP    |
| Creates the shipping order <br /> Prepare and print labels (also for reprinting)  <br /> No completion | /AEB/PA_PB_DLV_CR_PKGS_W_PRINT | /AEB/PA_PB_SHP_CR_PKGS_W_PRINT |

### Function module /AEB/PA_PB_DLV_CR_PKGS_W_PRINT

This function module creates a shipping order (if not already existing), with all the transmitted packages. Labels are  printed directly for these packages. If the packages are already existing, only labels are printed. If packaging information is required in Carrier Cloud, the parameter IM_VEKPS must not only include packages for which a label is to be printed, but also the packages that are below this package.

Import parameters:

* IM_LIKP (Header data shipment)
* IM_VEKPS (List of handling units)
* IM_VEPOS_WITH_VENUM_OF_VEKPS (list of packed items)
* IM_LIPPS (List of item data shipment)
* IM_VBPAS (List of partners)
* IM_IS_SHP_TO_CREATE (Forces the creation of a shipping order) If the parameter does not have value "X", a  
  shipping order will be created provided that no shipping order exists so far. If the parameter has value "X", an error message will be returned if the shipping order already has been created.
* IM_SUPPRESS_INFO_MSGS (Indicator for suppressing info messages)
* IM_SUPPRESS_ALL_MSGS (Indicator for suppressing all messages)

Export parameters:

* EX_HAS_ERROR (Error marker)
* EX_HAS_WARNING (Warning indicator)
* EX_MESSAGES (Messages)
* EX_PACKAGE_RESULTS (List of processed packages - result)

> The parameters are the similar for the function modules listed above and the explanation can be applied to them the same way.

## TM business objects

For TM based objects, the AEB add-on does provides a class and methods instead of function modules. However, the limitations regarding the flexibility mentioned above do apply here as well.

Class name: /AE1/CL_PA_PB_FRO_PS_AC

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Combined tasks
      </th>

      <th>
        Method
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        * Create the shipping order  - Prepare all labels    - Complete the shipment
      </td>

      <td>
        CREATE_SHP_W_COMPLETE
      </td>
    </tr>

    <tr>
      <td>
        * Create the shipping order   - Prepare all labels   - Complete the shipment
      </td>

      <td>
        CREATE_SHP_W_PRINT
      </td>
    </tr>

    <tr>
      <td>
        * Create the shipping order - No preparation, no print - No completion
      </td>

      <td>
        CREATE_SHP
      </td>
    </tr>

    <tr>
      <td>
        * Prepare and print labels - Complete the shipping order
      </td>

      <td>
        PRINT_SHP
      </td>
    </tr>

    <tr>
      <td>
        * Prepare labels - Complete the shipping order
      </td>

      <td>
        COMPLETE_SHP
      </td>
    </tr>

    <tr>
      <td>
        * Creates the shipping order (checks automatically, if not already existing)                                        - Prepare and print labels (also usuable for reprinting, if package already exists)                               - No completion
      </td>

      <td>
        CREATE_PKGS_W_PRINT
      </td>
    </tr>
  </tbody>
</Table>

# Creating a shipping order

In this example we're using an outbound delivery from SAP to create the shipping order in Carrier Cloud. The following code is based on a program where you enter the delivery number. Then it selects the required data from various tables and calls the function module:

```text Create shipping order
REPORT zaeb_create_first_shipment.

PARAMETERS: vbeln TYPE likp-vbeln.

DATA:
  likp  TYPE likp,
  lipss TYPE STANDARD TABLE OF lips,
  vbpas TYPE STANDARD TABLE OF vbpa,
  vekps TYPE STANDARD TABLE OF vekp,
  vepos TYPE STANDARD TABLE OF vepo.

SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.
SELECT * FROM lips INTO TABLE lipss WHERE vbeln = vbeln.
SELECT * FROM vbpas INTO TABLE vbpas WHERE vbeln = vbeln.
"Determine vekps for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB
"Determine vepos for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB

CALL FUNCTION '/AEB/PA_PB_DLV_CR_SHP'
  EXPORTING
    im_likp  = likp
    im_vekps = vekps
    im_lipss = lipss
    im_vbpas = vbapas
		im_vepos = vepos.
```

# Processing a shipping order

Processing the shipping order means something like adding packages or set the completion status. In this code sample we're processing a shipping order by printing labels and completing it.

```text
REPORT zaeb_print_and_complete_shipment.

PARAMETERS: vbeln TYPE likp-vbeln.

DATA:
  likp  TYPE likp.

SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.

CALL FUNCTION '/AEB/PA_PB_DLV_PRINT_SHP'
  EXPORTING
    im_likp  = likp.
```

# Canceling a shipping order

In some cases, it might be necessary to cancel the shipping order in Carrier Cloud to start over. Use one of the following functions to achieve this:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Business object
      </th>

      <th>
        Function module or class method
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Outbound delivery
      </td>

      <td>
        /AEB/PA_PB_DLV_CANCEL_SHP
      </td>
    </tr>

    <tr>
      <td>
        Shipment
      </td>

      <td>
        /AEB/PA_PB_SHP_CANCEL_SHP
      </td>
    </tr>

    <tr>
      <td>
        TM business object
      </td>

      <td>
        Class: /AE1/CL_PA_PB_FRO_PS_BC  
        Method: CANCEL_SHP
      </td>
    </tr>
  </tbody>
</Table>

Here is a code snippet that shows how to cancel a shiping order that was created based on a delivery from SAP:

```text
REPORT zaeb_cancel_shipment.

PARAMETERS: vbeln TYPE likp-vbeln.

DATA:
  likp  TYPE likp.

SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.

CALL FUNCTION '/AEB/PA_PB_DLV_CANCEL_SHP'
  EXPORTING
    im_likp  = likp.
```

# Creating a pickup

After completing the shipping order you might also want to create a pickup. This is possible using one of the following function modules:

| Business object | Function module                |
| :-------------- | :----------------------------- |
| Shipment        | /AEB/PA_PB_CR_PU_W_PR_FOR_SHPS |
| Delivery        | /AEB/PA_PB_CR_PU_W_PR_FOR_DLVS |
