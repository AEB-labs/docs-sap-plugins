---
title: Customs Inventory Management (CIM)
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
# BAdIs to change booking request data

When the booking request is triggered by the standard includes of the add-on, you can change the data by using the according BAdI for the SAP document:    

| SAP business object /  document | BAdI                 |
| :------------------------------ | :------------------- |
| Transfer order (WM)             | /AEB/AES_CIM_BR_TO_1 |
| Warehouse task (EWM)            | /AE1/AES_CIM_BR_WT_1 |

<br />

# BAdIs to change whether a sap document should be transferred as booking request

When the standard customizing settings are not suficiant to to determine whether a sap document is relevant for Customs Inventory Management and should be transferred you can override the standard determination (positive and negative) with the following BAdIs. 

| SAP business object /  document | BAdI                 |
| :------------------------------ | :------------------- |
| Transfer order (WM)             | /AEB/AES_CIM_BR_TO_2 |
| Warehouse task (EWM)            | /AE1/AES_CIM_BR_WT_2 |

# Call APIs of Customs Inventory Management

Using public class **/AEB/CL_AES_PB_CIM_BF** you can call the APIs of CIM directly from within your SAP system. The class is a direct representation of the CIM interfaces.  See below for some coding examples for the usage.  

## Creating a booking request from scratch

```Text Create booking request
DATA:
  cim_bf                    TYPE REF TO /aeb/cl_aes_pb_cim_bf,
  exception                 TYPE REF TO /aeb/cx_01_pb_bf_inv_sc,
  cr_br_req_do              TYPE REF TO /aeb/cl_aes_pb_cim_cr_br_rq_do,
  cr_br_res_do              TYPE REF TO /aeb/cl_aes_pb_cim_cr_br_rs_do,
  result_language_iso_codes TYPE /aeb/01_char2s,
  booking_request           TYPE REF TO /aeb/if_aes_pb_cim_br_req_do,
  object_factory            TYPE REF TO /aeb/cl_aes_pb_cim_br_do_fa,
  decisive_date             TYPE REF TO /aeb/if_aes_pb_cim_daz_do,
  timestamp                 TYPE timestampl,
  material                  TYPE REF TO /aeb/if_aes_pb_ic_material_do,
  person_in_charge          TYPE REF TO /aeb/if_aes_pb_cim_person_do,
  quantity                  TYPE REF TO /aeb/if_aes_pb_cim_quantity_do.

CREATE OBJECT object_factory.
cim_bf = /aeb/cl_aes_pb_cim_bf=>new_for( im_org_unit = 'DEFAULT' ).

booking_request = object_factory->new_aes_cim_br_req_do( ).
decisive_date = object_factory->new_aes_cim_daz_do( ).
decisive_date->/aeb/if_01_pb_date_and_zone_do~set_timezone( 'UTC' ).
GET TIME STAMP FIELD timestamp.
decisive_date->/aeb/if_01_pb_date_and_zone_do~set_date_in_timezone( CONV char22( timestamp ) ).
booking_request->set_decisive_date( decisive_date ).
booking_request->set_id( '12312432' ).
material = object_factory->new_aes_ic_material_do( ).
booking_request->set_material( material ).
person_in_charge = object_factory->new_aes_cim_person_do( ).
person_in_charge->set_forename( 'forename' ).
person_in_charge->set_surname( 'surname' ).
booking_request->set_person_in_charge( person_in_charge ).
booking_request->set_profile_code( 'PROFILE_CODE' ).
quantity = object_factory->new_aes_cim_quantity_do( im_unit = 'ST'
                                                    im_value = '1' ).
booking_request->set_quantity( quantity ).
booking_request->set_type( 'CUSTOMS_WAREHOUSE_TO_FREE_CIRCULATION' ). "also possible: 'CUSTOMS_WAREHOUSE_TO_RE_EXPORT'

CREATE OBJECT cr_br_req_do.
APPEND 'DE' TO result_language_iso_codes.
APPEND 'EN' TO result_language_iso_codes.
cr_br_req_do->set_result_language_iso_codes( result_language_iso_codes ).
cr_br_req_do->set_username( sy-uname ).
cr_br_req_do->set_client_system_id( sy-sysid && '_' && sy-mandt ).
cr_br_req_do->set_client_identcode( 'CLIENT' ).
cr_br_req_do->set_booking_request( booking_request ).

TRY.
    cr_br_res_do = cim_bf->create_booking_request( im_cr_br_req_do = cr_br_req_do ).
  CATCH /aeb/cx_01_pb_bf_inv_sc INTO exception.
    WRITE exception->/aeb/if_01_cx_message~get_msg_as_str( ).
ENDTRY.
```

## Retrieve stock account balances

```Text Get stock balances
DATA:
  cim_bf                    TYPE REF TO /aeb/cl_aes_pb_cim_bf,
  exception                 TYPE REF TO /aeb/cx_01_pb_bf_inv_sc,
  gsab_req_do               TYPE REF TO /aeb/cl_aes_pb_cim_gsab_req_do,
  gsab_res_do               TYPE REF TO /aeb/cl_aes_pb_cim_sab_res_do,
  result_language_iso_codes TYPE /aeb/01_char2s,
  account_keys              TYPE /aeb/01_char100s,
  stock_accounts_balances   TYPE /aeb/cl_aes_pb_cim_sabs_do=>tt_pb_cim_sabs_do.

cim_bf = /aeb/cl_aes_pb_cim_bf=>new_for( im_org_unit = 'DEFAULT' ).

CREATE OBJECT gsab_req_do.
APPEND 'DE' TO result_language_iso_codes.
APPEND 'EN' TO result_language_iso_codes.
gsab_req_do->set_result_language_iso_codes( result_language_iso_codes ).
gsab_req_do->set_username( sy-uname ).
gsab_req_do->set_client_system_id( sy-sysid && '_' && sy-mandt ).
gsab_req_do->set_client_identcode( 'CLIENT' ).
gsab_req_do->set_inventory_key( 'INVENTORY_XYZ' ).
APPEND '12345' TO account_keys.
gsab_req_do->set_stock_account_keys( account_keys ).

TRY.
    gsab_res_do = cim_bf->get_stock_account_balances( gsab_req_do ).
    stock_accounts_balances = gsab_res_do->get_stock_accounts_balances( ).
  CATCH /aeb/cx_01_pb_bf_inv_sc INTO exception.
    WRITE exception->/aeb/if_01_cx_message~get_msg_as_str( ).
ENDTRY.
```