---
title: Add additional Fields
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
If you want to use additional Fields to transfer your own data, you can do the following.

```text Add additional data
DATA:
  ae41cons_do  TYPE REF TO /aeb/if_ct_pb_ae41cons_do,
  fld_name_nv  TYPE REF TO /aeb/cl_01_string_nv,
  fld_value_nv TYPE REF TO /aeb/cl_01_string_nv,
  ae41cadfld   TYPE REF TO /aeb/if_ct_pb_ae41adfld_do,
  ae41cadflds  TYPE /aeb/if_ct_pb_ae41adfld_do=>tt_ae41adfld_do.

ae41cons_do = im_as4cons->get_ae41cons( ).
fld_name_nv = im_nullable_value_factory->string( 'CONS Field 1' ).
fld_value_nv = im_nullable_value_factory->string( 'CONS Value 1' ).
ae41cadfld = im_as4co_data_object_factory->new_ct_pb_ae41adfld_do( im_fld_name  = fld_name_nv
                                                                   im_fld_value = fld_value_nv ).
APPEND ae41cadfld TO ae41cadflds.
ae41cons_do->set_ae41cadfld( ae41cadflds ).
im_as4cons->set_ae41cons( ae41cons_do ).
```
