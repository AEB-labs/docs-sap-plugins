---
title: The first shipment
excerpt: Prepared calls for copy/paste in your development environment
deprecated: true
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Let's start right away, here is a complete call to create a shipment and receive the ususal response.

To make it easy, we use a SAP outbound delivery for the business object to create the shipment. The following code is based on a programm where you enter or provide a delivery number. Then it selects the required data from SAP  tables and creates the shipment.

```text Create first shipment
REPORT zaeb_create_first_shipment.

PARAMETERS: vbeln TYPE likp-vbeln.

DATA:
  likp  TYPE likp,
  lipss TYPE STANDARD TABLE OF lips,
  vbpas TYPE STANDARD TABLE OF vbpa,
  vekps TYPE STANDARD TABLE OF vekp.

SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.
SELECT * FROM lips INTO TABLE lipss WHERE vbeln = vbeln.
SELECT * FROM vbpas INTO TABLE vbpas WHERE vbeln = vbeln.
SELECT * FROM vekp INTO TABLE vekps WHERE vpobjkey = vbeln.

CALL FUNCTION '/AEB/PA_PB_DLV_CR_SHP'
  EXPORTING
    im_likp  = likp
    im_vekps = vekps
    im_lipss = lipss
    im_vbpas = vbapas.
```

Nothing more to do. Just execute this application, choose a delivery number und go on. Then the application will show you messages if there are missing configuration and so on. 

![836](https://files.readme.io/fac0249-2020-11-13_141451.jpg "2020-11-13_141451.jpg")

If you don't know how to fix the message, check our configuration guide.\
You have done it. You have created your first shipment.
