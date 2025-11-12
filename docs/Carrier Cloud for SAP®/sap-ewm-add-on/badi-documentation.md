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
All BAdIs can be found in the Enhancement Spot */SHIP/ES\_CCO*.

### /SHIP/EX\_CCO\_CARRIER\_DET

The *GET\_CARRIER\_DET* method can be used to implement a customised determination for the carrier, the service or the additional services, among other things.

### /SHIP/EX\_CCO\_GET\_SHIPMENTS

In the standard system, shipping orders are determined using the delivery order number. If this is not used as reference 1, the search parameters can be adjusted in the *CHANGE\_GET\_SHIP\_REQ* method.

### /SHIP/EX\_CCO\_CREATE\_SHIPMENT

The *CHANGE\_CREATED\_SHIPMENT\_REQ* method can be used to change all data of the *CREATE\_SHIPMENT* call. See [Carrier Connect API documentation](https://transport-freight-management.docs.developers.aeb.com/reference/).

### /SHIP/EX\_CCO\_PRINT\_DOCUMENT

The *PRINT\_DOCUMENT* method can be used to control the printing of documents at shipping order level. No printer determination for documents at shipping order level is supplied in the standard system. The fallback class */SHIP/CL\_CCO\_PRINT\_PDFDOCUMENT* is already implemented in this BAdI. This can be used as a reference for customised logic.

### /SHIP/EX\_CCO\_PRINT\_LABEL

The *PRINT\_LABEL* method can be used to control the printing of documents at package level. In the standard system, printer determination is delivered via the condition records. The fallback class */SHIP/CL\_CCO\_PRINT\_LABEL* is already implemented in this BAdI. This can be used as a reference for customised logic.

### /SHIP/EX\_CCO\_PROCESS\_SHIPMENT

All data of the *PROCESS\_SHIPMENT* call can be changed using the *CHANGE\_PROCESS\_SHIPMENT\_REQ* method. See [Carrier Connect API documentation](https://transport-freight-management.docs.developers.aeb.com/reference/).

### /SHIP/EX\_CCO\_VALIDATE\_SHIPMENT

All data of the *VALIDATE\_SHIPMENT* call can be changed using the *CHANGE\_VALIDATE\_SHIPMENT\_REQ* method. The BAdI is called in the *VALIDATE\_SHIPMENT* method of the */SHIP/CCO\_CL\_CARRIER* class. This must be customised.

### /SHIP/EX\_CCO\_WORKSTATION\_ID

The workstation can be changed using the *SET\_WORKSTATION\_ID* method. By default, the workstation is always a concatenation of the client and the EWM warehouse number.

### /SHIP/EX\_CCO\_ADD\_TRACKN\_IDENT

The *SET\_HU\_IDENT\_TRACKN* method can be used to influence the setting of the alternative HU identification (tracking number and master package identifier). The fallback class */SHIP/CL\_CCO\_ADD\_TRACKN\_IDENT* is already implemented in this BAdI. This can be used as a reference for customised logic.
