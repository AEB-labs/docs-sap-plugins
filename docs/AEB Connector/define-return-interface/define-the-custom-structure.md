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

```text Structure
- AE41CONS
  - CONS_NO
  - CLIENT_CDE
  - DESPATCHDATE
  - AS41CADFLD
    - FLD_NAME
    - FLD_VALUE
```

It contains a table named "AE41CONS". The table contains the string "CONS\_NO", the string "CLIENT\_CDE", the string "DESPATCHDATE" and the table "AS41CADFLD". With that knowledge we can create the following types:

```text Custom Structures
TYPES: BEGIN OF ts_as41cadfld,
         fld_name  TYPE string,
         fld_value TYPE string,
       END OF ts_as41cadfld.
TYPES tt_as41cadfld TYPE STANDARD TABLE OF ts_as41cadfld WITH DEFAULT KEY.
TYPES: BEGIN OF ts_ae41cons,
         cons_no      TYPE string,
         client_cde   TYPE string,
         despatchdate TYPE string,
         as41cadfld   TYPE tt_as41cadfld,
       END OF ts_ae41cons.
TYPES tt_ae41cons TYPE STANDARD TABLE OF ts_ae41cons WITH DEFAULT KEY.
TYPES: BEGIN OF ts_parameter,
         ae41cons TYPE tt_ae41cons,
       END OF ts_parameter.
```

We always have to start from the inside out. So first we need to define the structure for the table AS41CADFLD named ts\_as41cadfld. Then we need to create the table type tt\_as41cadfld from this structure. This table type is then included in the structure for the table AE41CONS named ts\_ae41cons. Again we have to create a table type out of the structure ts\_ae41cons named tt\_ae41cons. This is the whole structure we see completed, but we know that we receive a structure. So we have to create a structure around the table ae41cons in this case named ts\_parameter. This type is the exact type we receive from the ASSIST4 system.
