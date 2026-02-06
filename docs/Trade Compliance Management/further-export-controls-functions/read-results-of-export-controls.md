---
title: Read data from Export Controls and Risk Assessment
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
## Read data from Export Controls

If you want to read data of the export controls checks for subsequent processes, you can use one of the following functions:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Requirement
      </th>

      <th>
        Function module / Class
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Read certain data from the result of an export controls check. ( for a single item of a transaction)
      </td>

      <td>
        /AEB/CL_CMP_PB_EC_DATA_READ_BC
      </td>
    </tr>

    <tr>
      <td>
        Read the approval and clearing data for the items of a document.  
        The parameters IM_ORG_UNIT and IM_ENGN_CALL_PARAMS are optional. Both parameters will be determined by the functions itself if left empty.  
        The fields VBELN and POSNR of parameter IM_REQUESTS are deprecated. Use instead the fields DOC_NO and ITEM_REF_ID of the same structure. <br />  

        * This function calls a web service when executed.
      </td>

      <td>
        /AEB/CMP_PB_EC_GET_CLEA_FOR
      </td>
    </tr>

    <tr>
      <td>
        Read the history of comments that have been entered when an item was released.
      </td>

      <td>
        /AEB/CMP_PB_EC_GET_HISTORY_FOR
      </td>
    </tr>

    <tr>
      <td>
        Read the data for a questionnaire created by an export controls check.
      </td>

      <td>
        /AEB/CL_CMP_PB_QUEST_BC
      </td>
    </tr>

    <tr>
      <td>
        Read further informations about the license.

        * This function works only for licenses from license management (LM). But it cannot  be used for the general licenses from export controls. (meaning the licenses from the data service).  
        * This function calls a web service when executed.
      </td>

      <td>
        /AEB/CL_CMP_PB_EC_LIC_INFO_BC
      </td>
    </tr>
  </tbody>
</Table>

Assuming you have released the order 5050 , item 10 and want to read the licence data from the assigned licence. In this example, the licence statement is retrieved this way:

```text Read licence data assigned to a release
DATA:
  lic_info_bc   TYPE REF TO /aeb/cl_cmp_pb_ec_lic_info_bc,
  vbeln         TYPE /aeb/cmp_pb_doc_no,
  lic_result    TYPE REF TO /aeb/if_cmp_pb_ec_lici_res_do,
  lic_infos     TYPE /aeb/cmp_if_cmp_pb_ec_li_i_dos,
  lic_info      TYPE REF TO /aeb/if_cmp_pb_ec_lic_info_do,
  lic_statement TYPE string,
  jurisdiction  TYPE REF TO /aeb/if_cmp_pb_juri_do.

vbeln = '000005050'.
lic_info_bc = /aeb/cl_cmp_pb_ec_lic_info_bc=>new_for( vbeln ).
lic_result = lic_info_bc->get_license_info_for_item( '000010' ).
lic_infos = lic_result->get_lic_info_dos( ).
LOOP AT lic_infos INTO lic_info.
  jurisdiction = lic_info->get_jurisdiction( ).
  IF jurisdiction->get_ident_code( )->v = 'DE-EU-Plugin'. "or  'US-EAR-Plugin'
    lic_statement = lic_info->get_license_statement( ).
    EXIT.
  ENDIF.
ENDLOOP.
```

## Handle updates of questionnaires from Risk Assessment

To handle the update event of a questionnaire you can use the following BAdIs:

| Business object          | BAdI                |
| :----------------------- | :------------------ |
| Sales document           | /AEB/CMP_RA_SDOC_01 |
| Delivery                 | /AEB/CMP_RA_DLV_01  |
| Purchase document        | /AEB/CMP_RA_PD_01   |
| Service order            | /AEB/CMP_RA_SO_01   |
| Service transaction (S4) | /AE1/CMP_RA_ST_01   |

Let's implement a simple scenario were we update a Z-field of VBAK with the status of the questionnaire which is linked to the sales order.

> ❗️ If you want to use the result of an questionnaire in the export control check, e.g.  for the end usage or other manual restrictions,  do not use the “Handle Update” BAdI. You'll need to determine this data at runtime: see [Change data for Export Control checks](https://sap-plugins.docs.developers.aeb.com/docs/change-data-of-export-controls-check)  and description of the class  _/AEB/CL_CMP_PB_QUEST_BC_ on top of this page. As the result is returned to SAP asynchronously via  journal, it is otherwise possible that the result of a questionnaire that has just been completed has not yet been processed at the time of the check.

```text Update vbak when questionnaire update event occurs
DATA:
      questionnaire     TYPE REF TO /aeb/if_cmp_pb_quest_do,
      lcl_completed TYPE /aeb/01_boolean,
      total_result_type TYPE string.

    questionnaire = im_questionnaire_bc->get_questionnaire( ).

    IF questionnaire IS INITIAL.
      "Questionnaire was invalidated => no relevant results
      RETURN. "you might implement a reset here instead
    ENDIF.
    
    lcl_completed = questionnaire->get_is_completed( ).
    IF lcl_completed = 'X'.
      "if not completed: Old Questionnaire was invalidated but new incomplete version exists
      RETURN. "you might implement a reset here instead
    ENDIF.
    
    "getting the identcode for all critical results
    "DATA:
    "  critical_results TYPE /aeb/if_cmp_pb_quest_critr_do=>tt_quest_critr_do,
    "  critical_result TYPE zraqresults-z_raq_result.
    "critical_results = questionnaire->get_critical_results( ).
    "LOOP AT critical_results INTO critical_result.
    "  critical_result = critical_result->get_identcode( ).
    "  ...
    "ENDLOOP.
        
    total_result_type = questionnaire->get_total_result_type( ).

    UPDATE vbak SET zz_quest_res = total_result_type WHERE vbeln = im_vbeln.
```

Just use the passed parameter im_questionnaire_bc to get the questionnaire. Get the total result type and update the VBAK table. In this context the table is locked so no other process can change it.  
If you don't want to use a direct SQL update Statement to update your SAP document, you can use one of the released BAPIs, e.g. BAPI_SALESORDER_CHANGE that are provided by SAP. But then you have to consider certain things: Calling a BAPI will also run through the user exits or BAdIs for saving the document.  Usually the AEB includes for the online check are implemented there, which means an additional Compliance check is triggered when calling the SAP BAPI. In most cases, this check is not needed, as the AEB standard also does a re-check of the SAP document after the HDL_QUESTIONNAIRE_UPDATED BAdI is called.  
To prevent that the BAPI triggers the online check, use the public helper class /AEB/CL_CMP_PB_CHK_HP. This one is also an importing parameter of the BAdI method. The class has the method SUPPRESS_CHECK which has to be called before every BAPI call and the method RESET_SUPPRESS_CHECK which enables the check again for the next BAPI call. A recommended use of the BAPI in this BAdI implementation would therefore look like as follows:

```text Update with use of a BAPI
DATA:
    return           TYPE TABLE OF bapiret2,
    order_header_in  TYPE bapisdh1,
    order_header_inx TYPE bapisdh1x.

  im_public_check_helper->suppress_check( ).

  order_header_in-purch_no_c = '0815'.
  order_header_inx-purch_no_c = 'X'.
  order_header_inx-updateflag = 'U'.

  CALL FUNCTION 'BAPI_SALESORDER_CHANGE'
    EXPORTING
      salesdocument    = im_vbeln
      order_header_in  = order_header_in
      order_header_inx = order_header_inx
    TABLES
      return           = return.

  CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
    EXPORTING
      wait = 'X'.
  
  im_public_check_helper->reset_suppress_check( ).
```

> 🚧 Call the BAPI_TRANSACTION_COMMIT to ensure that the changed data can be used by the following Compliance check. Otherwise the check is not executed with the current data.

## Raise execptions during questionnaire updates

In case of an error during an questionnaire update you can raise an exception as follows:

```text Raise exception in BAdI when questionnaire update event occurs
raise EXCEPTION type /AEB/CX_cmp_pb_quest_sc
    		EXPORTING _badi_name = 'ZCL_IM_AEB_RA_SDOC_01'
                  _error_msg_string = 'Handle questionnaire not possible'
                  textid = /aeb/cx_cmp_pb_quest_sc=>hdl_questionnaire_upd_failed.
```
