---
title: Getting the count of questionnaires from Risk Assessment
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
If you use the Risk Assessment module in Trade Compliance Management and want to know how many questionnaires already exist you can use an according function in a BAdI implementation. Available BAdIs are listed as follows:

| SAP business object            | BAdI                    |
| :----------------------------- | :---------------------- |
| Sales document                 | /AEB/CMP\_EC\_ORDER\_06 |
| Delivery                       | /AEB/CMP\_EC\_DLV\_03   |
| Purchase document              | /AEB/CMP\_EC\_PD\_03    |
| Service orders (ERP ECC)       | /AEB/CMP\_EC\_SO\_03    |
| Service transactions (S/4HANA) | /AE1/CMP\_EC\_ST\_02    |

## Get count of questionnaires

```text Get number of questionnaires for a reference
DATA:
  quest_filter_do TYPE REF TO /aeb/if_cmp_pb_quest_filter_do,
  quest_count     TYPE /aeb/cmp_pb_cnt_of_quest,
  reference       TYPE  /aeb/01_char250.

quest_filter_do = im_data_object_factory->new_cmp_pb_ec_quest_filter_do( ).

quest_filter_do->set_companyname( 'ReadMe' ).
quest_filter_do->set_iscompleted( 'X' ).
reference = im_value->get_transaction_ref_comment( )->v.
quest_filter_do->set_reference( reference ).

IF NOT im_questionnaire_bc IS INITIAL.
	quest_count = im_questionnaire_bc->get_questionnaire_count_for( quest_filter_do ).
ENDIF.
```
```text Get number of questionnaires for company ReadMe of the last 6 Months
DATA:
  quest_filter_do   TYPE REF TO /aeb/if_cmp_pb_quest_filter_do,
  creationdate      TYPE REF TO /aeb/if_01_pb_dtirf_do,
  relative_interval TYPE REF TO /aeb/if_01_pb_rif_do,
  relative_from     TYPE REF TO /aeb/if_01_pb_rel_interval_do,
  quest_count       TYPE /aeb/cmp_pb_cnt_of_quest.
  
quest_filter_do = im_data_object_factory->new_cmp_pb_ec_quest_filter_do( ).

quest_filter_do->set_companyname( 'ReadMe' ).

creationdate = im_data_object_factory->new_01_pb_dtirf_do( ).
relative_interval = im_data_object_factory->new_01_pb_rif_do( ).
relative_from = im_data_object_factory->new_01_pb_rel_interval_do( ).
relative_from->set_duration( -6 ).
relative_from->set_unit( /aeb/if_01_pb_rel_interval_do=>c_unit_months ).
relative_interval->set_from( relative_from ).
creationdate->set_relativeinterval( relative_interval ).
quest_filter_do->set_creationdate( creationdate ).

IF NOT im_questionaire_bc IS INITIAL.
  quest_count = im_questionnaire_bc->get_questionnaire_count_for( quest_filter_do ).
ENDIF.
```

The call presets the filters to the values which are used when you open the overview application. They then are overwritten if you actually fill them. So if your result is not as expected, you can check the preset values if you open the application in Risk Assessment.
