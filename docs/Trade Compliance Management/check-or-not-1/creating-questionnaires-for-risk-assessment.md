---
title: Creating questionnaires for Risk Assessment
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
      slug: getting-the-count-of-questionnaires-for-a-filter
      title: Getting the count of questionnaires from Risk Assessment
---
Using the Risk Assessment module in Trade Compliance Management you can create questionnaires automatically when checking your documents with export controls. 

Functional process: 

- A questionnaire needs to be requested as part of the export control check. See example bewlo for th e required data  
- When a new document is created in SAP, the number of the document is not yet known at the time of the first check. Therefore, the questionnaire cannot be created immediately. 
- Instead, a journal entry is created and processed in the background by a job. This triggers the export control check for the document again and creates the questionnaire. 

To provide the required data, implement the according BAdI:

- Sales documents:  /AEB/CMP_EC_ORDER_06
- Deliveries:  /AEB/CMP_EC_DLV_03
- Purchase documents: /AEB/CMP_EC_PD_03
- Service orders: /AEB/CMP_EC_SO_03
- Service transactions (S/4HANA): /AE1/CMP_EC_ST_02 

This coding example creates a questionnaire 

```text Create questionnaire
DATA:
        quest_parm      TYPE REF TO /aeb/if_cmp_pb_ec_cr_quest_do,
        additional_info TYPE REF TO /aeb/cl_01_string_nv,
        template_id     TYPE REF TO /aeb/cl_01_string_nv,
        department      TYPE REF TO /aeb/cl_01_string_nv,
        email           TYPE REF TO /aeb/cl_01_string_nv,
        user            TYPE REF TO /aeb/cl_01_string_nv,
        org_unit        TYPE string,
        org_unit_nv     TYPE REF TO /aeb/cl_01_string_nv.

      additional_info = im_nullable_value_factory->string( 'some additional information' ).
      template_id = im_nullable_value_factory->string( '000025' ).
      department = im_nullable_value_factory->string( 'Department' ).
      email = im_nullable_value_factory->string( 'E@ma.il' ).
      user = im_nullable_value_factory->string( 'User Name' ).
      org_unit = im_context_bc->get_org_unit( ).
      org_unit_nv = im_nullable_value_factory->string( org_unit ).

      quest_parm = im_data_object_factory->new_cmp_pb_ec_cr_quest_do(
        im_additional_info = additional_info
        im_department      = department
        im_email_address   = email
        im_template_id     = template_id
        im_user_name       = user
        im_org_unit        = org_unit_nv ).

      im_value->set_create_questionnaire_prm( quest_parm ).
```
```text Create questionnaire using creator information
TYPE-POOLS: szadr.
      DATA:
        adr_sel       TYPE addr1_sel,
        adr           TYPE addr1_val,
        vbak          TYPE vbak,
        ernam         TYPE ernam,
        user_address  TYPE usaddress,
        adr3_complete TYPE szadr_addr3_complete,
        addr3_line    TYPE szadr_addr3_line,
        s_department  TYPE string,
        adsmtp_line   TYPE szadr_adsmtp_line,
        s_email       TYPE string,
        user_name     TYPE string,
        quest_parm    TYPE REF TO /aeb/if_cmp_pb_ec_cr_quest_do,
        additional_info TYPE REF TO /aeb/cl_01_string_nv,
        template_id   TYPE REF TO /aeb/cl_01_string_nv,
        department    TYPE REF TO /aeb/cl_01_string_nv,
        email         TYPE REF TO /aeb/cl_01_string_nv,
        user          TYPE REF TO /aeb/cl_01_string_nv,
        org_unit        TYPE string,
        org_unit_nv     TYPE REF TO /aeb/cl_01_string_nv.

      vbak = im_context_bc->get_vbak( ). "needs to be replaced for other business object data
      ernam = vbak-ernam.

      CALL FUNCTION 'SUSR_USER_READ'
        EXPORTING
          user_name    = ernam
        IMPORTING
          user_address = user_address.

      CALL FUNCTION 'ADDR_PERS_COMP_GET_COMPLETE'
        EXPORTING
          addrnumber     = user_address-addrnumber
          persnumber     = user_address-persnumber
        IMPORTING
          addr3_complete = adr3_complete.

      LOOP AT adr3_complete-addr3_tab INTO addr3_line.
        IF s_department IS INITIAL
          AND addr3_line-data-department IS NOT INITIAL.
          s_department = addr3_line-data-department.
          EXIT.
        ENDIF.
      ENDLOOP.

      LOOP AT adr3_complete-adsmtp_tab INTO adsmtp_line.
        IF s_email IS INITIAL
          AND adsmtp_line-adsmtp-smtp_addr IS NOT INITIAL.
          s_email = adsmtp_line-adsmtp-smtp_addr.
          EXIT.
        ENDIF.
      ENDLOOP.

      additional_info = im_nullable_value_factory->string( 'some additional information' ).
      template_id = im_nullable_value_factory->string( '000001' ).
      department = im_nullable_value_factory->string( s_department ).
      email = im_nullable_value_factory->string( s_email ).
      user_name = ernam.
      user = im_nullable_value_factory->string( user_name ).
      org_unit = im_context_bc->get_org_unit( ).
      org_unit_nv = im_nullable_value_factory->string( org_unit ).

      quest_parm = im_data_object_factory->new_cmp_pb_ec_cr_quest_do(
        im_additional_info = additional_info
        im_department      = department
        im_email_address   = email
        im_template_id     = template_id
        im_user_name       = user
        im_org_unit        = org_unit_nv ).

      im_value->set_create_questionnaire_prm( quest_parm ).
```

If you want to block the document until the questionnaire is processed, activate the risk assessment integration in the compliance profile of Trade Compliance Management.

There might be the requirement to add the result of the questionnaire to the export controls check. Good to know - at the moment  you request the creation of a questionnaire you also have the possibilty to get the questionnaire data back. 

Let's implement a very common case. The questionnaire does have the information about the end usage of the document items.  In this example we assume that there is only one critical result which holds the end usage of the transaction:

```text Read the data of the questionnaire
DATA:
        items                          TYPE /aeb/if_cmp_pb_ec_item_do=>tt_ec_item_do,
        curr_item                      TYPE REF TO /aeb/if_cmp_pb_ec_item_do,
        questionnaire_do               TYPE REF TO /aeb/if_cmp_pb_quest_do,
        curr_crit_res                  TYPE REF TO /aeb/if_cmp_pb_quest_critr_do,
        quest_parm                     TYPE REF TO /aeb/if_cmp_pb_ec_cr_quest_do,
        additional_info                TYPE REF TO /aeb/cl_01_string_nv,
        template_id                    TYPE REF TO /aeb/cl_01_string_nv,
        department                     TYPE REF TO /aeb/cl_01_string_nv,
        email                          TYPE REF TO /aeb/cl_01_string_nv,
        user                           TYPE REF TO /aeb/cl_01_string_nv,
        initial_string_nv              TYPE REF TO /aeb/cl_01_string_nv,
        final_usage_from_questionnaire TYPE REF TO /aeb/cl_01_char_255_nv,
        org_unit                       TYPE string,
        org_unit_nv                    TYPE REF TO /aeb/cl_01_string_nv,
        tmp_char255                    TYPE /aeb/01_char255.

      additional_info = im_nullable_value_factory->string( 'some additional information' ).
      template_id = im_nullable_value_factory->string( '000008' ).
      department = im_nullable_value_factory->string( 'Sales Department' ).
      email = im_nullable_value_factory->string( 'sales@company.com' ).
      user = im_nullable_value_factory->string( 'User Name' ).
      org_unit = im_context_bc->get_org_unit( ).
      org_unit_nv = im_nullable_value_factory->string( org_unit ).

      quest_parm = im_data_object_factory->new_cmp_pb_ec_cr_quest_do(
        im_additional_info = additional_info
        im_department      = department
        im_email_address   = email
        im_template_id     = template_id
        im_org_unit        = org_unit_nv
        im_user_name       = user ).
      im_value->set_create_questionnaire_prm( quest_parm ).

      IF NOT im_questionnaire_bc IS INITIAL.  "this is an optional paramter, not every context allows to getQuestionaire
        questionnaire_do = im_questionnaire_bc->get_questionnaire_for( im_questionnaire_template_id = template_id->v
                                                                       im_reference_id_host = im_value->get_transaction_ref_id( )->v ).

        READ TABLE questionnaire_do->get_critical_results( ) INTO curr_crit_res INDEX 1.
        IF curr_crit_res IS NOT INITIAL.
          tmp_char255 = curr_crit_res->get_identcode( ).
          final_usage_from_questionnaire = im_nullable_value_factory->char_255( tmp_char255 ).
          items = im_value->get_items( ).
          LOOP AT items INTO curr_item.
            curr_item->set_final_usages( final_usage_from_questionnaire ).
          ENDLOOP.
        ENDIF.
      ENDIF.
```

Always check if the parameter "im_questionnaire_bc" is initial. Because this parameter is not passed in every context, but during an export control check it is available.  
The next step is to get the data of the questionnaire with the method "get_questionnaire_for". Pass the parameter for the templateID and the referenceIdHost to retrieve the questionnaire. To stick with our example scenario, read the final usage from the result of the questionnaire and set the final usage for each item in the export controls data.  
The method getQuestionnaireFor might raise an exception in case of an error. It is not necessary to catch this exception as it will be handled outside. 

In addition to the logic above there is also the possibility to save the data locally in your SAP system once the update event of a questionnaire has been synchronized.