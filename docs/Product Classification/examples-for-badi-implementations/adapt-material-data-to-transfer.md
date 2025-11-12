---
title: Adapt data to transfer
excerpt: BAdI /AEB/TA_MATERIAL_02
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The following site will assist you how to adapt the data of a material which is transferred to Product Classification. But before we start, some things that are good to know:

**Access to MAR-tables, org units, etc**

If you want to use contextual data,  e.g. the material tables (MARA, MARC..) or if you like to have the relevant organizational units,  use the parameter "IM_MATERIAL_CONTEXT". This parameter has some methods, e.g. get_marc to read the content of the MARx-tables. This includes the content which is not yet persistent. Example:

DATA (lt_marcs) = im_material_context->get_marcs( ).

**Empty values vs. null **

 Some of our fields with simple data types like char or string are implemented as so called _nullable values_. So we have a class representation of each simple data type.  
If you change such a field you need to use the correct type. For the material number you have to use the class /AEB/CL_01_CHAR_50_NV. NV means nullable values. To create this nullable value, the BAdI method offers you the parameter im_nullable_value_factory. Each nullable value has the attribte v for value.  
Why do we use nullable values? To differentiate between whether or not an empty value will lead  to an update in Product Classification. In case you want an update, create the nullable value without assign a value to v. If there should be no update, set  the field value in the interface to "NULL".

Let us start with adapting the alternative material number. The standard just fill the alternative material number with the external represenation of the SAP material no.

```text Read and/or change a single field value
DATA:
    matnr_nv  TYPE REF TO /aeb/cl_01_char_50_nv,
    matnr     TYPE matnr,
    alt_matnr TYPE REF TO /aeb/cl_01_char_50_nv.

  matnr_nv = im_value->get_material_no( ).
  matnr = matnr_nv->v.
  
  IF matnr = 'M-11'.
    alt_matnr = im_nullable_value_factory->char_50( 'M-11-TEST' ).
    im_value->set_alt_material_no( alt_matnr ).  
  ENDIF.
```

Okay, know to a more complex requirement, where you want to adapt the value of a classification. In the following example we will change the value of COCO_IMPORT_DE (commodity code for import) to a const value.  
First you have to loop over the classification values. Then you have to check it the value is the COCO_IMPORT_DE. In this case we change it to a fixed value of '01022959310'.  

```text Change the value of a classification
DATA:
    classification_values TYPE /aeb/if_ta_pb_cls_value_v1_do=>tt_pb_cls_value_v1_do,
    current_cls_value     TYPE REF TO /aeb/if_ta_pb_cls_value_v1_do,
    type_name             TYPE /aeb/01_char255.

  classification_values = im_value->get_classifications_values_v1( ).
  LOOP AT classification_values INTO current_cls_value.
    type_name = current_cls_value->get_type_name( ).
    IF type_name = 'COCO_IMPORT_DE'.
      current_cls_value->set_value( '01022959310' ).
      current_cls_value->set_processing_comment( 'make it hard coded' ).
      current_cls_value->set_approved( 'X' ).
      current_cls_value->set_valid_from( sy-datum ).
      current_cls_value->set_valid_to( '40000101' ).
    ENDIF.
  ENDLOOP.
```

As you see the whole material is implemented object-oriented. So in order to access the classifications you have to call the method get_classifications.  
How to add a classification? See the example below.

```text Add a classification
DATA:
    classification_values TYPE /aeb/if_ta_pb_cls_value_v1_do=>tt_pb_cls_value_v1_do,
    new_cls_value         TYPE REF TO /aeb/if_ta_pb_cls_value_v1_do.

  classification_values = im_value->get_classifications_values_v1( ).
  new_cls_value = im_data_object_factory->new_ta_pb_cls_value_v1_do( im_type_name = 'COCO_IMPORT_DE' ).

  new_cls_value->set_value( '01022959310' ).
  new_cls_value->set_processing_comment( 'make it hard coded' ).
  new_cls_value->set_approved( 'X' ).
  new_cls_value->set_valid_from( sy-datum ).
  new_cls_value->set_valid_to( '40000101' ).
  
  append new_cls_value to classification_values.
  im_value->set_classification_values_v1( classification_values ).
```

Here you can see that we use the im_data_object_factory to create a new value. Ok let us try another thing. If you like to add a certificate to a material, you have to create a certificate by uinge the parameter im_data_object_factory which is provided by the  BAdI method. Then you fill the needed values and add the certificate to the existing list and set the to the material. The example below shows the needed steps. 

```text Add an certificate to material
DATA:
    certificate     TYPE REF TO /aeb/if_ta_pb_cert_type_do,
    certificates    TYPE /aeb/if_ta_pb_certificate_do=>tt_certificate_do,
    new_certificate TYPE REF TO /aeb/if_ta_pb_certificate_do,
    cert_data       TYPE REF TO /aeb/if_ta_pb_cert_data_do,
    cert_type       TYPE REF TO /aeb/if_ta_pb_cert_type_do.

  new_certificate = im_data_object_factory->new_ta_pb_certificate_do( ).
  new_certificate->set_client_sys_certificate_id( 'SAP_E01_400' ).

  cert_data = im_data_object_factory->new_ta_pb_cert_data_do( ).
  cert_data->set_additional_explanation( 'add_expl' ).
  cert_data->set_date_of_issue( sy-datum ).
  cert_data->set_detail( 'detail' ).
  cert_data->set_end_of_validity( sy-datum ).
  cert_data->set_geoid( 'US' ).
  cert_data->set_reference( 'reference' ).
  new_certificate->set_data( cert_data ).

  cert_type = im_data_object_factory->new_ta_pb_cert_type_do( ).
  cert_type->set_code( 'Y921' ).
  cert_type->set_country_of_certificate( 'DE' ).
  cert_type->set_division( '1' ).
  cert_type->set_qualifier( 'Q01' ).
  cert_type->set_valid_from( sy-datum ).
  cert_type->set_valid_to( '40000101' ).
  new_certificate->set_type( cert_type ).

  certificates = im_value->get_certificates( ).
  APPEND new_certificate TO certificates.
  im_value->set_certificates( certificates ).
```

And here another example which shows how to add attachments to your material.  
Please note that this example uses fronted service. This won't work in productiv implementation, but for testing it will work. 

```text Add an attachment to the material (selection only for front end uses)
DATA:
    attachment      TYPE REF TO /aeb/if_ta_pb_mat_att_do,
    file_name_nv    TYPE REF TO /aeb/cl_01_char_250_nv,
    attachment_data TYPE /aeb/01_rawstring,
    description_nv  TYPE REF TO /aeb/cl_01_string_nv,
    encoding_nv     TYPE REF TO /aeb/cl_01_char_50_nv,
    property        TYPE REF TO /aeb/if_ta_pb_mat_prop_do,
    mime_type_nv    TYPE REF TO /aeb/cl_01_char_50_nv,
    lt_file_table   TYPE filetable,
    lv_file         TYPE string,
    lv_rc           TYPE i,
    file_manager    TYPE REF TO cl_file_system_manager,
    string          TYPE string,
    lt_rawtab       TYPE TABLE OF char255.

  CALL METHOD cl_gui_frontend_services=>file_open_dialog
    EXPORTING
      window_title      = 'Select file'
      file_filter       = ''
      initial_directory = ''
      multiselection    = ''
    CHANGING
      file_table        = lt_file_table
      rc                = lv_rc.
        
  READ TABLE lt_file_table INTO lv_file INDEX 1.
  CALL METHOD cl_gui_frontend_services=>gui_upload
  EXPORTING
    filename = lv_file
  CHANGING
    data_tab = lt_rawtab.
      
  READ TABLE lt_rawtab INTO string INDEX 1.
  CALL FUNCTION 'SCMS_STRING_TO_XSTRING'
    EXPORTING
      text   = string
    IMPORTING
      buffer = attachment_data.

  file_name_nv = im_nullable_value_factory->char_250( 'Testfile' ).
  description_nv = im_nullable_value_factory->string( 'This is just a test' ).
  encoding_nv = im_nullable_value_factory->char_50( 'UFT-8' ).
  mime_type_nv = im_nullable_value_factory->char_50( 'text/plain' ).

  attachment = im_data_object_factory->new_ta_pb_mat_att_do( 
  im_file_name              = file_name_nv
  im_attachment_data        = attachment_data
  im_attachment_description = description_nv
  im_encoding               = encoding_nv
  im_mime_type              = mime_type_nv ).
    
  im_value->add_attachment( attachment ).
```
```text Add a link as attachment
  DATA:
    link_attachment  TYPE REF TO /aeb/if_ta_pb_att_link_do,
    link_attachments TYPE /aeb/if_ta_pb_att_link_do=>tt_pb_att_link_do.

  link_attachment = im_data_object_factory->new_ta_pb_att_link_do( ).
  link_attachment->set_name( 'Description of the link' ).
  link_attachment->set_target_url( 'https://www.aeb.com/en' ).
  APPEND link_attachment TO link_attachments.

  im_value->set_link_attachments( link_attachments ).
```

To add additional goods properties to your material you have to use method add_property.

```text Add property
lv_id = 'PROPERTY_ID'.
lv_value = 'PROPERTY_VALUE'.

lo_property = im_data_object_factory->new_ta_pb_mat_prop_do( im_identcode = lv_id
                                                             im_value     = lv_value ).
im_value->add_property( im_value = lo_property ).
```