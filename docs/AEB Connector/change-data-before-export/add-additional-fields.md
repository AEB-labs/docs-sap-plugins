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
[block:code]
{
  "codes": [
    {
      "code": "DATA:\n  ae41cons_do  TYPE REF TO /aeb/if_ct_pb_ae41cons_do,\n  fld_name_nv  TYPE REF TO /aeb/cl_01_string_nv,\n  fld_value_nv TYPE REF TO /aeb/cl_01_string_nv,\n  ae41cadfld   TYPE REF TO /aeb/if_ct_pb_ae41adfld_do,\n  ae41cadflds  TYPE /aeb/if_ct_pb_ae41adfld_do=>tt_ae41adfld_do.\n\nae41cons_do = im_as4cons->get_ae41cons( ).\nfld_name_nv = im_nullable_value_factory->string( 'CONS Field 1' ).\nfld_value_nv = im_nullable_value_factory->string( 'CONS Value 1' ).\nae41cadfld = im_as4co_data_object_factory->new_ct_pb_ae41adfld_do( im_fld_name  = fld_name_nv\n                                                                   im_fld_value = fld_value_nv ).\nAPPEND ae41cadfld TO ae41cadflds.\nae41cons_do->set_ae41cadfld( ae41cadflds ).\nim_as4cons->set_ae41cons( ae41cons_do ).",
      "language": "text",
      "name": "Add additional data"
    }
  ]
}
[/block]