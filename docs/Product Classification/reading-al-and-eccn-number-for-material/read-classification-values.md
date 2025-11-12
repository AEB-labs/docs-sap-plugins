---
title: Read classifications
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: read-classifications
      title: Read classification certificates
---
You have two options to read classifications:

* get a value by type name
* get a value by profile

The following example shows how to use the option to get the value by type name. In addition to the type name you have to provide the decisive date for which the classification is valid. 

```text Get value by type name
DATA:
      bc    TYPE REF TO /aeb/cl_ta_pb_smat_read_v1_bc,
      value TYPE REF TO /aeb/if_ta_pb_sync_cls_val_do.

    bc  = /aeb/cl_ta_pb_smat_read_v1_bc=>new_for( im_material_no = 'M-11'
                                                  im_material_plant = '1010' ).
    
    value = bc->get_classification_value( im_decisive_date = sy-datum
                                          im_type_name = 'EXPORT_CTRL_EU' ).
    
    IF value IS NOT INITIAL. 
      WRITE value->get_processing_comment( ).
      WRITE value->get_type_name( ).
      WRITE value->get_value( ).
    ENDIF.
```

And now let us get the value by profile. In addition to the type name you get an alias which could be used to make the classification understandable for other systems.

```text Get values by profile
DATA:
  bc     TYPE REF TO /aeb/cl_ta_pb_smat_read_v1_bc,
  value  TYPE REF TO /aeb/if_ta_pb_sync_cls_valp_do,
  values TYPE /aeb/if_ta_pb_sync_cls_valp_do=>tt_sync_cls_valp_do.

bc  = /aeb/cl_ta_pb_smat_read_v1_bc=>new_for( im_material_no = 'M-11'
                                              im_material_plant = '1010' ).

values = bc->get_classification_values( im_decisive_date = sy-datum
                                        im_profile = 'DE_EXPORT_CONTROL_CHECK' ).
LOOP AT values INTO value.
  WRITE value->get_processing_comment( ).
  WRITE value->get_type_name( ).
  WRITE value->get_value( ).
  WRITE value->get_alias( ).
ENDLOOP.
```
