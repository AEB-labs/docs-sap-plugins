---
title: How the check processes in the EWM add-on
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
For analyzing the process around the integration of Carrier Connect in EWM, there are several options.

# Logging and Monitoring

Following capabilities are provided for logging and monitoring:

1. The application logs for called proxy methods can be viewed in transaction _SLG1_ with the object _/SCWM/WME_ and the subobject _/SHIP/CCO_.
2. To monitor the shipments either use the transaction _/SHIP/CCO_COCKPIT_ or open the Carrier Cloud for SAP interface.
3. Use transaction _SPPFP_ to monitor the PPF processing logs.
4. There are processing protocols in the AEB Carrier Connect Shipment.
5. Active the Webservice Trace via transaction _SRT_UTIL_

# Useful Debugging

To debug different useful methods, use the checkpoint group _/SHIP/CCO_ in transaction _SAAB_.

## Scheduling Condition

The scheduling condition evaluation is done in class _/SHIP/CL_IM_SC_SHIP_LABEL_ in method _IF_EX_EVAL_SCHEDCOND_PPF~EVALUATE_SCHEDULE_CONDITION_.

The check is positive if the returning parameter rc = 0.

## Shipment creation

To analyze the data for the shipment creation, go to class _/SHIP/CCO_CL_CARRIER_ in method _SET_SHIPMENT_.  
There are two different structures. The first structure (request) contains the data sent from the SAP system.

The second structure (response) contains the answer from the webservice call:

![](https://files.readme.io/98e05c148774db6334c11127e9a3618cec5dfebfde62967ca928e149670af185-image.png)

## Printer determination

The printer determination is done in class _/SHIP/CCOCLCARRIER_ in method _PRINT_LABEL_.