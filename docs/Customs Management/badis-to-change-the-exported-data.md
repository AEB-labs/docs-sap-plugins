---
title: BAdIs to change the exported data
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: >-
    The following sections provide some code snippets to demonstrate changing of
    the data.
  pages:
    - type: basic
      slug: customs_change_data
      title: Change determined data
    - type: basic
      slug: customs_add_new_data
      title: Add new data
    - type: basic
      slug: example-coding-for-deliveries
      title: Adding country specific extensions
---
| SAP business object /  document | BAdI                 |
| :------------------------------ | :------------------- |
| Delivery                        | /AEB/AES_CONS_SDV_01 |
| Shipment                        | /AEB/AES_CONS_SHP_01 |
| Invoice                         | /AEB/AES_CONS_INV_01 |
| Purchase document               | /AEB/AES_CONS_PD_01  |
| Incoming Invoice                | /AEB/AES_CONS_II_01  |
| Freight order                   | /AE1/AES_CONS_FRO_01 |
| Material document               | /AEB/AES_CONS_MD_01  |

The data sent to Customs Management can be changed using the according BAdI for the SAP business object as listed in the table above. Each BAdI offers an interface method AFTER_STD_FILLING with the parameter IM_VALUE.   This parameter contains the data determined by the standard logic of the plug-in.  The data in this structure will be sent to Customs Management eventually, so you need to change it in the BAdI implementation according to your requirements. 

This simple example shows how you can change a single field value in the IM_VALUE structure: 

```Text Change a single value
DATA:
    remark TYPE /aeb/if_aes_pb_consignment_do=>t_remark.

  CONCATENATE 'Invoice number: ' im_vbrk-vbeln ', invoice date: ' im_vbrk-fkdat
         INTO remark
         SEPARATED BY space.
  im_value->set_remark( remark ).
```

The IM_VALUE parameter is a structure of type /AEB/IF_AES_PB_CONSIGNMENT_DO, which represents a consignment of  Customs Management. It contains different sub-objects: 

- Deliveries
- Items
- Packages 
- Parties

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a7b3b0e-image.png",
        null,
        ""
      ],
      "align": "center",
      "sizing": "1px"
    }
  ]
}
[/block]


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/fdc0de1-image.png",
        null,
        ""
      ],
      "align": "center",
      "sizing": "1px"
    }
  ]
}
[/block]


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2613e46-image.png",
        null,
        ""
      ],
      "align": "center",
      "sizing": "50% "
    }
  ]
}
[/block]


So regardless of what kind of SAP business object triggered the data transfer, Customs Management is always receiving a consignment object containing the mentioned sub-objects. Some examples:

| SAP business object transfered to AEB | SAP level                  | Interface object |
| :------------------------------------ | :------------------------- | :--------------- |
| Invoice                               | Invoice                    | Consignment      |
| -"-                                   | Invoice                    | Delivery         |
| -"-                                   | Invoice items              | Items            |
| Shipment                              | Shipment                   | Consignment      |
| -"-                                   | Deliveries of the shipment | Deliveries       |
| -"-                                   | Delivery items             | Items            |
| -"-                                   | Handling units             | Packages         |
| Outbound delivery                     | Outbound delivery          | Consignment      |
| -"-                                   | Outbound delivery          | Deliveries       |
| -"-                                   | Outbound delivery items    | Items            |
| -"-                                   | Handling units             | Packages         |
| Purchase order                        | Purchase order             | Consignment      |
|                                       | Purchase order             | Delivery         |
|                                       | Purchase order items       | Items            |
| etc. etc.                             |                            |                  |