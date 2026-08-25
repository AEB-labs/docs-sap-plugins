---
title: Carrier Event Service - tracking events
deprecated: false
hidden: false
metadata:
  robots: index
---
To open the list of tracking events in Carrier Connect for a certain document, use one of the following classes.

| SAP document  | Class                    |
| :------------ | :----------------------- |
| Delivery      | /AEB/CL_PA_PB_DLV_DLS_AC |
| Shipment      | /AEB/CL_PA_PB_SHP_DLS_AC |
| Freight order | /AE1/CL_PA_PB_FRO_DLS_AC |

Code example:

```
DATA:
public_ac_class TYPE REF TO /AEB/CL_MA_PB_DLV_AF_AC. 

public_ac_class = /AEB/CL_MA_PB_DLV_AF_AC=>new_for( im_likp_vbeln =
```
