---
title: Display tracking events and other overviews
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

  For Carrier Event Service, different functions are provided for this purpose. See the according section in the API guide for "Carrier Cloud For SAP" for more details.&#x20;
</Callout>

You can use various functions of the add-on to open web views showing overviews provided by Monitoring & Alerting.

# Open overviews for an SAP delivery&#x20;

Use the class /AEB/CL_MA_PB_DLV_AF_AC and the according methods to open different kind of overviews:

- SHOW_CONSIGNMENT_EVENT_LIST - shows the events of an Monitoring & Alerting shipment
- SHOW_HANDLING_UNIT_EVENT_LIST - shows the events of a handling unit
- SHOW_ORDER_EVENT_LIST - shows the events of an Monitoring & Alerting order
- SHOW_CONSIGNMENT_OVERVIEW -shows an overview of an Monitoring & Alerting shipment
- SHOW_HANDLING_UNIT_OVERVIEW - shows an overview of a handling unit
- SHOW_ORDER_OVERVIEW - shows an overview of an Monitoring & Alerting order

Note: Referencing packages does not work if the same package is transferred to Monitoring & Alerting multiple times. Example: You have transferred an SAP outbound delivery including a certain package #1234. Now you transfer the SAP shipment associated with the delivery. If the delivery package #1234 has not been packaged into another package of the shipment,  it is created two times in Monitoring & Alerting.

Sample coding:

```text Open overview
Data:
dlv_af_ac type ref to /aeb/cl_ma_pb_dlv_af_ac.

dlv_af_ac = /aeb/cl_ma_pb_dlv_af_ac=>new_for(
im_likp_vbeln = 0012345678 ).

dlv_af_ac->show_consignment_overview( ).
```

<br />

# Open the event overview for an SAP delivery

Use the function module /AEB/MA_PB_OPEN_EV_LIST_DLV  to display the events of an SAP delivery in a web view. The function module includes an authorization check that depends on the organizational unit. To start the function, a user must have the authorization ZAEB_MAE01 for the corresponding organizational unit.

The function module has the following import parameters:&#x20;

- IM_LIKP_VBELN - document number of delivery
- IM_VKORG - sales organization of delivery
- IM_BUKRS (Optional) - company code of the delivery. If left empty, the company code is determined through the sales organization
- IM_IS_TO_SHOW_AS_FULLSCREEN - indicates whether the events should be displayed in full-screen mode or window mode (“X” = full screen, “-” = window)

<br />

# Open overviews for an SAP freight order&#x20;

Use the class /AE1/CL_MA_PB_FRO_AF_AC and the according methods to open different kind of overviews:

- SHOW_CONSIGNMENT_EVENT_LIST - shows the events of an Monitoring & Alerting shipment
- SHOW_CONSIGNMENT_OVERVIEW -shows an overview of an Monitoring & Alerting shipment
- SHOW_HANDLING_UNIT_EVENT_LIST - shows the events of a handling unit
- SHOW_HANDLING_UNIT_OVERVIEW - shows an overview of a handling unit

# &#x20;

# Open overviews for an SAP shipment

Use the class /AEB/CL_MA_PB_SHP_AF_AC and the according methods to open different kind of overviews:

- SHOW_CONSIGNMENT_EVENT_LIST - shows the events of an Monitoring & Alerting shipment
- SHOW_HANDLING_UNIT_EVENT_LIST - shows the events of a handling unit
- SHOW_ORDER_EVENT_LIST - shows the events of an Monitoring & Alerting order
- SHOW_CONSIGNMENT_OVERVIEW -shows an overview of an Monitoring & Alerting shipment
- SHOW_HANDLING_UNIT_OVERVIEW - shows an overview of a handling unit
- SHOW_ORDER_OVERVIEW - shows an overview of an Monitoring & Alerting order

Sample coding:

```text Open overview
Data:
shp_af_ac type ref to /aeb/cl_ma_pb_shp_af_ac.

shp_af_ac = /aeb/cl_ma_pb_shp_af_ac=>new_for(
im_tknum = <shipment number> ).

shp_af_ac->show_consignment_overview( ).
```

<br />

# Open the event overview for an SAP shipment

Use the function module /AEB/MA_PB_OPEN_EV_LIST_SHP to display the events of an SAP shipment in a web view. The function module includes an authorization check that depends on the organizational unit. To start the function, a user must have the authorization ZAEB_MA06 for the corresponding organizational unit.

The function module has the following import parameters:

- IM_VTTK_TKNUM - document number of the shipment<br />IM_BUKRS - company code of delivery<br />IM_IS_TO_SHOW_AS_FULLSCREEN - indicates whether the events should be displayed in full-screen mode or window mode (“X” = full screen, “-” = window)

# Open overviews for an SAP sales document

Use the class /AEB/CL_MA_PB_SDOC_AF_AC and the according methods to open different kind of overviews:

- SHOW_CONSIGNMENT_EVENT_LIST - shows the events of an Monitoring & Alerting shipment
- SHOW_HANDLING_UNIT_EVENT_LIST - shows the events of a handling unit
- SHOW_ORDER_EVENT_LIST - shows the events of an Monitoring & Alerting order
- SHOW_CONSIGNMENT_OVERVIEW -shows an overview of an Monitoring & Alerting shipment
- SHOW_HANDLING_UNIT_OVERVIEW - shows an overview of a handling unit
- SHOW_ORDER_OVERVIEW - shows an overview of an Monitoring & Alerting order

Sample coding:

```text Open overview
Data:
sdoc_af_ac type ref to /aeb/cl_ma_pb_sdoc_af_ac.

sdoc_af_ac = /aeb/cl_ma_pb_sdoc_af_ac=>new_for(
im_vbak_vbeln = <sales document number> ).

sdoc_af_ac->show_order_overview( ).

```
