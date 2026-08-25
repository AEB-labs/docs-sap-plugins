---
title: 'Display tracking events '
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

# Open event list for an SAP delivery

Use the function module /AEB/MA_PB_OPEN_EV_LIST_DLV  to display the events of an SAP delivery in a web view. The function module includes an authorization check that depends on the organizational unit. To start the function, a user must have the authorization ZAEB_MAE01 for the corresponding organizational unit.

The function module has the following import parameters:&#x20;

- IM_LIKP_VBELN (Document number of delivery)
- IM_VKORG (Sales organization of delivery)
- IM_BUKRS(Optional) (Company code of delivery)
- IM_IS_TO_SHOW_AS_FULLSCREEN (Indicates whether the events should be displayed in full-screen mode or window mode (“X” = full screen, “-” = window))
