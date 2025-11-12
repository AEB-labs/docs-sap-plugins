---
title: Available Attributes
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
The private class attribute *GS\_ATTRIBUTES* of the class */SHIP/CCO\_CL\_CARRIER* contains various already-read data, which can be read using the method */SHIP/CCOCL\_CARRIER ->GET\_ATTRIBUTES*. The */SHIP/CCO\_CL\_CARRIER ->SET\_ATTRIBUTES* method can be used to set the attributes and also to fill customer-specific attributes.

<Callout icon="💡" theme="default">
  ### Please note that the "Get method" should always be called before calling the "Set method".
</Callout>

### MANUAL:

This field can be used to identify that the coding was called manually and not via the PPF action. The *DATA\_PREPARE* method of the */SHIP/EX\_ESI* BAdI sets this indicator if the importing parameter is maintained accordingly.

### DELV\_HEADER\_TAB:

The *DELV\_HEADER\_TAB* table is read in the fallback class of the */SHIP/EX\_CCO\_GET\_DELIVERY* BAdI using a standard method (*/SCWM/CL\_DLV\_MANAGEMENT\_PRD->QUERY*).\
This attribute contains the delivery header information of the calling HU.

### DELV\_ITEM\_TAB:

The *DELV\_ITEM\_TAB* table is read in the fallback class of the */SHIP/EX\_CCO\_GET\_DELIVERY* BAdI using a standard method (*/SCWM/CL\_DLV\_MANAGEMENT\_PRD->QUERY*).\
This table contains the delivery item information of the calling HU.

### HU\_HEADERS\_TAB:

The *HU\_HEADERS\_TAB* table is read in the fallback class of the */SHIP/EX\_CCO\_GET\_DELIVERY* BAdI via a standard method (*/SCWM/CL\_DLV\_MANAGEMENT\_PRD->QUERY*). This table contains the HU header information of the calling HU.

### HU\_ITEMS\_TAB:

The *HU\_ITEMS\_TAB* table is read in the fallback class of the */SHIP/EX\_CCO\_GET\_DELIVERY* BAdI using a standard method (*/SCWM/CL\_DLV\_MANAGEMENT\_PRD->QUERY*). This table contains the HU position information of the calling HU.

### DELV\_ITEM\_S:

A relevant delivery item is written in the *DELV\_ITEM\_S* structure.

### LOGH:

The log handle can be added to the *CONSTRUCTOR* method and is used for logging.

### WORKSTATION\_ID:

The client ID and the warehouse number are written in the *WORKSTATION\_ID*. It is transferred as the workstation for the shipment.

### EXPORT:

EXPORT is set to either "true" or "false" depending on whether the delivery is an export.

### CONSTANTS:

All entries are read from the database table */SHIP/CCO\_CONST* and written to the *CONSTANTS* table. 

### CARRIER\_DET\_TAB:

All entries are read from the database table */SHIP/CARRIERDET* and written to the table *CARRIER\_DET\_TAB*.

### SCU:

Contains the supply chain unit in which the HU is located.

### CARRIER\_DET\_S:

In the *GET\_CARRIER\_DET* method, an entry is written from the *CARRIER\_DET\_TAB* table to the *CARRIER\_DET\_S* structure to match the standard key factors warehouse number, forwarding agent and delivery conditions.

### SHIP\_EXISTING:

*SHIP\_EXISTING* is set to "true" in the *SET\_SHIPMENT* method if a shipping order already exists for the HU.

### SCHED\_COND:

This structure is set in the fallback class of the BAdI */SHIP/EX\_CCO\_GET\_SCHED\_COND\_RE* using the condition records maintained in transaction */SCWM/PRHU6*. The set work centre is written to the Terminal field in table */SCWM/ESI\_HDR*.

### DG\_RELEVANT:

Can be used to mark a delivery as relevant for hazardous goods. Is not filled by default.

### HU\_QVALUE:

Can be used to transfer the dangerous goods Q value of an HU. Is not filled by default.
