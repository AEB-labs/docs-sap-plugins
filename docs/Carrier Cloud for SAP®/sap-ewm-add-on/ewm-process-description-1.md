---
title: Process description - trigger the integration from EWM
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
With the completion of the HU packaging process step, the scheduling condition */SHIP/CL\_IM\_SC\_SHIP\_LABEL* is triggered.

If the result is positive (rc = 0) the PPF-Action will be executed.

The purpose of the next component is to generate a shipping order. For this purpose, the parameters from the SAP EWM are mapped for the Carrier Cloud for SAP web service methods.

After the request from the EWM system has been sent to Carrier Cloud for SAP, the response is processed. If the shipping request has been created successfully, this is stored in the SAP standard table */SCWM/ESI\_HDR*:

<Image align="center" src="https://files.readme.io/c272d4d263f65d84efa1849beafd15e71eb4cffe071e53f162e3e0a19f84ccd3-EWM_Architecture.png" />
