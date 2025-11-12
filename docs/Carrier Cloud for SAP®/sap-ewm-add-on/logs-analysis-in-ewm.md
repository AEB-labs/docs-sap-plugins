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

1. The application logs for called proxy methods can be viewed in transaction *SLG1* with the object */SCWM/WME* and the subobject */SHIP/CCO*.
2. To monitor the shipments either use the transaction */SHIP/CCO\_COCKPIT* or open the Carrier Cloud for SAP interface.
3. Use transaction *SPPFP* to monitor the PPF processing logs.
4. There are processing protocols in the AEB Carrier Connect Shipment.
5. Active the Webservice Trace via transaction *SRT\_UTIL*

# Useful Debugging

To debug different useful methods, use the checkpoint group */SHIP/CCO* in transaction *SAAB*.

## Scheduling Condition

The scheduling condition evaluation is done in class */SHIP/CL\_IM\_SC\_SHIP\_LABEL* in method *IF\_EX\_EVAL\_SCHEDCOND\_PPF\~EVALUATE\_SCHEDULE\_CONDITION*.

The check is positive if the returning parameter rc = 0.

## Shipment creation

To analyze the data for the shipment creation, go to class */SHIP/CCO\_CL\_CARRIER* in method *SET\_SHIPMENT*.\
There are two different structures. The first structure (request) contains the data sent from the SAP system.

The second structure (response) contains the answer from the webservice call:

![](https://files.readme.io/98e05c148774db6334c11127e9a3618cec5dfebfde62967ca928e149670af185-image.png)

## Printer determination

The printer determination is done in class */SHIP/CCOCLCARRIER* in method *PRINT\_LABEL*.
