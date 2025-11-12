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
The private class attribute _GS_ATTRIBUTES_ of the class _/SHIP/CCO_CL_CARRIER_ contains various already-read data, which can be read using the method _/SHIP/CCOCL_CARRIER ->GET_ATTRIBUTES_. The _/SHIP/CCO_CL_CARRIER ->SET_ATTRIBUTES_ method can be used to set the attributes and also to fill customer-specific attributes.

> 💡 Please note that the "Get method" should always be called before calling the "Set method".

### MANUAL:

This field can be used to identify that the coding was called manually and not via the PPF action. The _DATA_PREPARE_ method of the _/SHIP/EX_ESI_ BAdI sets this indicator if the importing parameter is maintained accordingly.

### DELV_HEADER_TAB:

The _DELV_HEADER_TAB_ table is read in the fallback class of the _/SHIP/EX_CCO_GET_DELIVERY_ BAdI using a standard method (_/SCWM/CL_DLV_MANAGEMENT_PRD->QUERY_).  
This attribute contains the delivery header information of the calling HU.

### DELV_ITEM_TAB:

The _DELV_ITEM_TAB_ table is read in the fallback class of the _/SHIP/EX_CCO_GET_DELIVERY_ BAdI using a standard method (_/SCWM/CL_DLV_MANAGEMENT_PRD->QUERY_).  
This table contains the delivery item information of the calling HU.

### HU_HEADERS_TAB:

The _HU_HEADERS_TAB_ table is read in the fallback class of the _/SHIP/EX_CCO_GET_DELIVERY_ BAdI via a standard method (_/SCWM/CL_DLV_MANAGEMENT_PRD->QUERY_). This table contains the HU header information of the calling HU.

### HU_ITEMS_TAB:

The _HU_ITEMS_TAB_ table is read in the fallback class of the _/SHIP/EX_CCO_GET_DELIVERY_ BAdI using a standard method (_/SCWM/CL_DLV_MANAGEMENT_PRD->QUERY_). This table contains the HU position information of the calling HU.

### DELV_ITEM_S:

A relevant delivery item is written in the _DELV_ITEM_S_ structure.

### LOGH:

The log handle can be added to the _CONSTRUCTOR_ method and is used for logging.

### WORKSTATION_ID:

The client ID and the warehouse number are written in the _WORKSTATION_ID_. It is transferred as the workstation for the shipment.

### EXPORT:

EXPORT is set to either "true" or "false" depending on whether the delivery is an export.

### CONSTANTS:

All entries are read from the database table _/SHIP/CCO_CONST_ and written to the _CONSTANTS_ table. 

### CARRIER_DET_TAB:

All entries are read from the database table _/SHIP/CARRIERDET_ and written to the table _CARRIER_DET_TAB_.

### SCU:

Contains the supply chain unit in which the HU is located.

### CARRIER_DET_S:

In the _GET_CARRIER_DET_ method, an entry is written from the _CARRIER_DET_TAB_ table to the _CARRIER_DET_S_ structure to match the standard key factors warehouse number, forwarding agent and delivery conditions.

### SHIP_EXISTING:

_SHIP_EXISTING_ is set to "true" in the _SET_SHIPMENT_ method if a shipping order already exists for the HU.

### SCHED_COND:

This structure is set in the fallback class of the BAdI _/SHIP/EX_CCO_GET_SCHED_COND_RE_ using the condition records maintained in transaction _/SCWM/PRHU6_. The set work centre is written to the Terminal field in table _/SCWM/ESI_HDR_.

### DG_RELEVANT:

Can be used to mark a delivery as relevant for hazardous goods. Is not filled by default.

### HU_QVALUE:

Can be used to transfer the dangerous goods Q value of an HU. Is not filled by default.