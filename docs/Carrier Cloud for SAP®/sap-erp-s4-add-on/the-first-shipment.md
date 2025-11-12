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
[block:code]
{
  "codes": [
    {
      "code": "REPORT zaeb_create_first_shipment.\n\nPARAMETERS: vbeln TYPE likp-vbeln.\n\nDATA:\n  likp  TYPE likp,\n  lipss TYPE STANDARD TABLE OF lips,\n  vbpas TYPE STANDARD TABLE OF vbpa,\n  vekps TYPE STANDARD TABLE OF vekp.\n\nSELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.\nSELECT * FROM lips INTO TABLE lipss WHERE vbeln = vbeln.\nSELECT * FROM vbpas INTO TABLE vbpas WHERE vbeln = vbeln.\nSELECT * FROM vekp INTO TABLE vekps WHERE vpobjkey = vbeln.\n\nCALL FUNCTION '/AEB/PA_PB_DLV_CR_SHP'\n  EXPORTING\n    im_likp  = likp\n    im_vekps = vekps\n    im_lipss = lipss\n    im_vbpas = vbapas.",
      "language": "text",
      "name": "Create first shipment"
    }
  ]
}
[/block]
Nothing more to do. Just execute this application, choose a delivery number und go on. Then the application will show you messages if there are missing configuration and so on. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/fac0249-2020-11-13_141451.jpg",
        "2020-11-13_141451.jpg",
        836,
        623,
        "#dfdfdf"
      ]
    }
  ]
}
[/block]
If you don't know how to fix the message, check our configuration guide.
You have done it. You have created your first shipment.