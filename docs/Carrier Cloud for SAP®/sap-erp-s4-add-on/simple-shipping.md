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

> 📘 To handle the shipping with complete flexibility, use the classes and methods documented in [Advanced Shipping](https://sap-plugins.docs.developers.aeb.com/docs/processing-more-in-detail).

# Supported business objects

Using the provided functions modules, you can transfer the following SAP business objects to Carrier Cloud for creating or updating a shipping order there: 

- Outbound delivery 
- Shipment (LE-TRA)
- TM business objects: freight order, freight booking and consignment

# Pre-determined shipping scenarios

## Outbound deliveries and shipments

[block:parameters]
{
  "data": {
    "h-0": "Combined tasks",
    "h-1": "FM for outbound deliveries",
    "h-2": "FM for shipments",
    "0-0": "Create the shipping order <br> Prepare all labels <br> Complete the shipping order <br>",
    "0-1": "/AEB/PA_PB_DLV_CR_SHP_W_COMP",
    "0-2": "/AEB/PA_PB_SHP_CR_SHP_W_COMP",
    "1-0": "Create the shipping order <br> Prepare and print all labels <br> Complete the shipping order",
    "1-1": "/AEB/PA_PB_DLV_CR_SHP_W_PRINT",
    "1-2": "/AEB/PA_PB_SHP_CR_SHP_W_PRINT",
    "2-0": "Create the shipping order <br> No preparation, no print <br> No completion",
    "2-1": "/AEB/PA_PB_DLV_CR_SHP",
    "2-2": "/AEB/PA_PB_SHP_CR_SHP",
    "3-0": "Prepare and print labels <br> Complete the shipping order",
    "3-1": "/AEB/PA_PB_DLV_PRINT_SHP",
    "3-2": "/AEB/PA_PB_SHP_PRINT_SHP",
    "4-0": "Prepare labels <br> Complete the shipping order",
    "4-1": "/AEB/PA_PB_DLV_COMPLETE_SHP",
    "4-2": "/AEB/PA_PB_SHP_COMPLETE_SHP",
    "5-0": "Creates the shipping order <br> Prepare and print labels (also for reprinting)  <br> No completion",
    "5-1": "/AEB/PA_PB_DLV_CR_PKGS_W_PRINT",
    "5-2": "/AEB/PA_PB_SHP_CR_PKGS_W_PRINT"
  },
  "cols": 3,
  "rows": 6,
  "align": [
    "left",
    "left",
    "left"
  ]
}
[/block]


### Function module /AEB/PA_PB_DLV_CR_PKGS_W_PRINT

This function module creates a shipping order (if not already existing), with all the transmitted packages. Labels are  printed directly for these packages. If the packages are already existing, only labels are printed. If packaging information is required in Carrier Cloud, the parameter IM_VEKPS must not only include packages for which a label is to be printed, but also the packages that are below this package.

  Import parameters:

- IM_LIKP (Header data shipment)
- IM_VEKPS (List of handling units)
- IM_VEPOS_WITH_VENUM_OF_VEKPS (list of packed items)
- IM_LIPPS (List of item data shipment)
- IM_VBPAS (List of partners)
- IM_IS_SHP_TO_CREATE (Forces the creation of a shipping order) If the parameter does not have value "X", a  
  shipping order will be created provided that no shipping order exists so far. If the parameter has value "X", an error message will be returned if the shipping order already has been created.
- IM_SUPPRESS_INFO_MSGS (Indicator for suppressing info messages)
- IM_SUPPRESS_ALL_MSGS (Indicator for suppressing all messages)

Export parameters:

- EX_HAS_ERROR (Error marker)
- EX_HAS_WARNING (Warning indicator)
- EX_MESSAGES (Messages)
- EX_PACKAGE_RESULTS (List of processed packages - result)

> The parameters are the similar for the function modules listed above and the explanation can be applied to them the same way.  

## TM business objects

For TM based objects, the AEB add-on does provides a class and methods instead of function modules. However, the limitations regarding the flexibility mentioned above do apply here as well.

Class name: /AE1/CL_PA_PB_FRO_PS_AC  

| Combined tasks                                                                                                                                                                                                                                        | Method                |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------- |
| - Create the shipping order  - Prepare all labels    - Complete the shipment                                                                                                                                                                          | CREATE_SHP_W_COMPLETE |
| - Create the shipping order   - Prepare all labels   - Complete the shipment                                                                                                                                                                          | CREATE_SHP_W_PRINT    |
| - Create the shipping order - No preparation, no print - No completion                                                                                                                                                                                | CREATE_SHP            |
| - Prepare and print labels - Complete the shipping order                                                                                                                                                                                              | PRINT_SHP             |
| - Prepare labels - Complete the shipping order                                                                                                                                                                                                        | COMPLETE_SHP          |
| - Creates the shipping order (checks automatically, if not already existing)                                        - Prepare and print labels (also usuable for reprinting, if package already exists)                               - No completion | CREATE_PKGS_W_PRINT   |

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

[block:parameters]
{
  "data": {
    "h-0": "Business object",
    "h-1": "Function module or class method",
    "0-0": "Outbound delivery",
    "0-1": "/AEB/PA_PB_DLV_CANCEL_SHP",
    "1-0": "Shipment",
    "1-1": "/AEB/PA_PB_SHP_CANCEL_SHP",
    "2-0": "TM business object",
    "2-1": "Class: /AE1/CL_PA_PB_FRO_PS_BC  \nMethod: CANCEL_SHP"
  },
  "cols": 2,
  "rows": 3,
  "align": [
    "left",
    "left"
  ]
}
[/block]


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