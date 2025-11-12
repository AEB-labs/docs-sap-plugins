---
title: Tracking event overview
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
To open the list of tracking events in Carrier Connect for a certain document, use one of the following classes.

| SAP document  | Class                         |
| :------------ | :---------------------------- |
| Delivery      | /AEB/CL\_PA\_PB\_DLV\_DLS\_AC |
| Shipment      | /AEB/CL\_PA\_PB\_SHP\_DLS\_AC |
| Freight order | /AE1/CL\_PA\_PB\_FRO\_DLS\_AC |

Code example:

```
DATA:
public_ac_class TYPE REF TO /AEB/CL_MA_PB_DLV_AF_AC. 

public_ac_class = /AEB/CL_MA_PB_DLV_AF_AC=>new_for( im_likp_vbeln = '0080000123' ).
public_ac_class->SHOW_CONSIGNMENT_EVENT_LIST( ).
```
