---
title: Display tracking event overview
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
<Callout icon="📘" theme="info">
  ### Carrier Event Service

  Different funtcions are provided in the context of Carrier Event Service. See the according section in the API guide fro "Carrier Cloud For SAP" for more details.&#x20;
</Callout>

<br />

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

public_ac_class = /AEB/CL_MA_PB_DLV_AF_AC=>new_for( im_likp_vbeln = '0080000123' ).
public_ac_class->SHOW_CONSIGNMENT_EVENT_LIST( ).
```
