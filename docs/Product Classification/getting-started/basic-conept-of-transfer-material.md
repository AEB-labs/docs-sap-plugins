---
title: Basic concept of transfer material
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: >-
    Like to learn more about basic concepts? Or start with adapting the transfer
    of materials?
  pages:
    - type: basic
      slug: basic-concept-of-read-material
      title: Basic concept of read material
    - type: basic
      slug: examples-for-badi-implementations
      title: Adapt the transfer of materials
---
The transfer of a material is splitted several parts. The following diagram shows those parts.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/cf94c92-2023-02-27_164805.jpg",
        "2023-02-27_164805.jpg",
        871,
        846,
        "#000000"
      ]
    }
  ]
}
[/block]

[block:parameters]
{
  "data": {
    "h-0": "Things to do",
    "h-1": "Example",
    "h-2": "Configurable?",
    "0-0": "Determine organizational unit",
    "0-1": "Can be assigned by plant or by sales org or just by client.",
    "0-2": "Yes",
    "1-0": "Determine engine client",
    "1-1": "The engine client is assigned to the organizational unit, but it is possible to assign the same engine client to multiple organizational units.",
    "1-2": "Yes",
    "2-0": "Determine product group",
    "2-1": "Product group can be assigned by material type and material category.",
    "2-2": "Yes",
    "h-3": "Custom Code",
    "0-3": "No",
    "1-3": "No",
    "2-3": "Yes",
    "3-0": "Collect material data",
    "3-1": "Provides all collected data which is to be transfered for the  material.",
    "3-2": "Yes",
    "3-3": "Yes",
    "4-0": "Check classification values",
    "4-1": "The plug-in sends only  classification values again, if there were changes since the last update. Otherwise existing changes in Product Classification could be overwritten.",
    "4-2": "Yes, /AEB/TA_WORK_IN_PROCESS_INIT has a flag where you disable/enable this behavior. In context of saving materials it is enabled by default. You cannot change this.",
    "4-3": "No",
    "5-0": "Check is material to be deleted",
    "5-1": "The material exists in SAP, but should not exist in Product Classification.",
    "5-2": "No",
    "5-3": "No",
    "6-0": "Transfer material data",
    "6-1": "The web service is called and the result will be handled, logs written etc.",
    "6-2": "No",
    "6-3": "No"
  },
  "cols": 4,
  "rows": 7
}
[/block]