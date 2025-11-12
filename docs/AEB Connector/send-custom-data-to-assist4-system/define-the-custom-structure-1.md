---
title: Define the custom structure
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Lets assume we have a Structure like this:
[block:code]
{
  "codes": [
    {
      "code": "- AE41CONS\n  - CONS_NO\n  - CLIENT_CDE\n  - DESPATCHDATE\n  - AS41CADFLD\n    - FLD_NAME\n    - FLD_VALUE",
      "language": "text",
      "name": "Structure"
    }
  ]
}
[/block]
It contains a table named "AE41CONS". The table contains the string "CONS_NO", the string "CLIENT_CDE", the string "DESPATCHDATE" and the table "AS41CADFLD". With that knowledge we can create the following types:
[block:code]
{
  "codes": [
    {
      "code": "TYPES: BEGIN OF ts_as41cadfld,\n         fld_name  TYPE string,\n         fld_value TYPE string,\n       END OF ts_as41cadfld.\nTYPES tt_as41cadfld TYPE STANDARD TABLE OF ts_as41cadfld WITH DEFAULT KEY.\nTYPES: BEGIN OF ts_ae41cons,\n         cons_no      TYPE string,\n         client_cde   TYPE string,\n         despatchdate TYPE string,\n         as41cadfld   TYPE tt_as41cadfld,\n       END OF ts_ae41cons.\nTYPES tt_ae41cons TYPE STANDARD TABLE OF ts_ae41cons WITH DEFAULT KEY.\nTYPES: BEGIN OF ts_parameter,\n         ae41cons TYPE tt_ae41cons,\n       END OF ts_parameter.",
      "language": "text",
      "name": "Custom Structures"
    }
  ]
}
[/block]
We always have to start from the inside out. So first we need to define the structure for the table AS41CADFLD named ts_as41cadfld. Then we need to create the table type tt_as41cadfld from this structure. This table type is then included in the structure for the table AE41CONS named ts_ae41cons. Again we have to create a table type out of the structure ts_ae41cons named tt_ae41cons. This is the whole structure we see completed, but we know that we send a structure. So we have to create a structure around the table ae41cons in this case named ts_parameter. This type is the exact type we need to send to the ASSIST4 system.