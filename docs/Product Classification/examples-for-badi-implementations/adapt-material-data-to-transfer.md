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
The following section explains how to adapt material data that is transferred to Product Classification. Before proceeding, there are a few important concepts to understand.

**Access to MAR-tables, org units, etc**

To access material-related data, for example from tables such as MARA and MARC, or to retrieve relevant organizational unit data, use the parameter `IM_MATERIAL_CONTEXT`. This parameter provides several methods, such as `GET_MARCS`, which retrieves the current content of the MARC structure, including data that has not yet been persisted to the database. Example:  DATA (lt_marcs) = im_material_context->get_marcs( ).

**Empty values vs. null**

Some fields with simple data types, such as CHAR or STRING, are implemented as so-called nullable values. For these fields, a dedicated class representation exists for each data type.

When modifying such fields, you must use the appropriate nullable type. For example, for the material number, use the class `/AEB/CL_01_CHAR_50_NV` (where “NV” stands for nullable value). To create an instance of a nullable value, use the parameter `IM_NULLABLE_VALUE_FACTORY` provided by the BAdI method. Each nullable value contains the attribute `V`, which holds the actual value.

Nullable values are used to distinguish whether an empty value should trigger an update in Product Classification. If an update is required, create the nullable value without assigning a value to attribute `V`. If no update should occur, set the corresponding field in the interface explicitly to `NULL`.

Now, let us look at adapting the alternative material number. In the standard implementation, the alternative material number is filled with the external representation of the SAP material number.

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

## Material properties

To add additional goods properties to your material you have to use method add_property.

### Classification 4.0.1

```text Add property
lv_id = 'PROPERTY_ID'.
lv_value = 'PROPERTY_VALUE'.

lo_property = im_data_object_factory->new_ta_pb_mat_prop_do( im_identcode = lv_id
                                                             im_value     = lv_value ).
im_value->add_property( im_value = lo_property ).
```

### Classification 4.0.2

```
DATA: lt_values TYPE /aeb/01_strings,
          lv_value  TYPE /aeb/01_string
          .
    lv_value = 'ABC'.
    APPEND lv_value to lt_values.
    lo_property = im_data_object_factory->new_ta_pb_mat_prop_do(
                    im_identcode       = 'TEST'
                    im_values          = lt_values
                  ).
    im_value->add_property( im_value = lo_property ).
```
