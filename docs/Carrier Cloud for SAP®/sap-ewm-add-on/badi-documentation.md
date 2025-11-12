---
title: BAdI Documentation
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
All BAdIs can be found in the Enhancement Spot _/SHIP/ES_CCO_.

### /SHIP/EX_CCO_CARRIER_DET

The _GET_CARRIER_DET_ method can be used to implement a customised determination for the carrier, the service or the additional services, among other things.

### /SHIP/EX_CCO_GET_SHIPMENTS

In the standard system, shipping orders are determined using the delivery order number. If this is not used as reference 1, the search parameters can be adjusted in the _CHANGE_GET_SHIP_REQ_ method.

### /SHIP/EX_CCO_CREATE_SHIPMENT

The _CHANGE_CREATED_SHIPMENT_REQ_ method can be used to change all data of the _CREATE_SHIPMENT_ call. See [Carrier Connect API documentation](https://transport-freight-management.docs.developers.aeb.com/reference/).

### /SHIP/EX_CCO_PRINT_DOCUMENT

The _PRINT_DOCUMENT_ method can be used to control the printing of documents at shipping order level. No printer determination for documents at shipping order level is supplied in the standard system. The fallback class _/SHIP/CL_CCO_PRINT_PDFDOCUMENT_ is already implemented in this BAdI. This can be used as a reference for customised logic.

### /SHIP/EX_CCO_PRINT_LABEL

The _PRINT_LABEL_ method can be used to control the printing of documents at package level. In the standard system, printer determination is delivered via the condition records. The fallback class _/SHIP/CL_CCO_PRINT_LABEL_ is already implemented in this BAdI. This can be used as a reference for customised logic.

### /SHIP/EX_CCO_PROCESS_SHIPMENT

All data of the _PROCESS_SHIPMENT_ call can be changed using the _CHANGE_PROCESS_SHIPMENT_REQ_ method. See [Carrier Connect API documentation](https://transport-freight-management.docs.developers.aeb.com/reference/).

### /SHIP/EX_CCO_VALIDATE_SHIPMENT

All data of the _VALIDATE_SHIPMENT_ call can be changed using the _CHANGE_VALIDATE_SHIPMENT_REQ_ method. The BAdI is called in the _VALIDATE_SHIPMENT_ method of the _/SHIP/CCO_CL_CARRIER_ class. This must be customised.

### /SHIP/EX_CCO_WORKSTATION_ID

The workstation can be changed using the _SET_WORKSTATION_ID_ method. By default, the workstation is always a concatenation of the client and the EWM warehouse number.

### /SHIP/EX_CCO_ADD_TRACKN_IDENT

The _SET_HU_IDENT_TRACKN_ method can be used to influence the setting of the alternative HU identification (tracking number and master package identifier). The fallback class _/SHIP/CL_CCO_ADD_TRACKN_IDENT_ is already implemented in this BAdI. This can be used as a reference for customised logic.