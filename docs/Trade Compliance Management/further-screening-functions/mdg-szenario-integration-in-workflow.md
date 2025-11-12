---
title: MDG scenario -  Integration in workflow
excerpt: Synchronous check function for Business partner
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
For the integration in a SAP MDG System we provide some functions to implement a compliance check for business partners from the workflow. 

Let us start with the check out of an workflow. For this case we provide the class /AEB/CL_CMP_PB_SYNC_CHK_BC. 

The first step is to ensure that in the workflow the 2 final steps of the workflow saving the change request as a business partner and replicating it to the target systems are separated into two steps.  

Then, between saving and replicating, the business partner check can be called via this class. It is  
important that at that time the business partner has already been saved. The check use the data from the business partner tables. Not from the workflow/the change request. 

In case the result of the check is critical workflow has to wait until the Compliance result handling is completed (Until resulthandling BADI is called). In case an error occurs IT has to check for the reason of the error and user has to manually restart the current workflow (Check business partner has to be called again).

```text Call the check and handle the result
PARAMETERS: bu_prt TYPE bu_partner.
PARAMETERS: chg_req TYPE /aeb/cl_cmp_pb_sync_chk_bc=>t_mdg_chg_req_no. "MDG Change Request number"

DATA:
  result      TYPE REF TO /aeb/if_cmp_pb_chk_res_do,
  chk_res_dos TYPE /aeb/if_cmp_pb_ou_chk_res_do=>tt_ou_chk_res_do,
  chk_res_do  TYPE REF TO /aeb/if_cmp_pb_ou_chk_res_do,
  sync_chk_bc TYPE REF TO /aeb/cl_cmp_pb_sync_chk_bc,
  exc         TYPE REF TO /aeb/cx_01_abs_sc,
  parm_exc    TYPE REF TO /aeb/cx_01_pb_missing_parm_sc.

sync_chk_bc = /aeb/cl_cmp_pb_sync_chk_bc=>new( ).
TRY.
    result = sync_chk_bc->check_business_partner( im_partner_number = bu_prt
                                         im_mdg_chg_req_no = chg_req  ).

    WRITE result->get_result_as_string( ).

    /aeb/cl_01_msgbox=>information( im_msg_txt = result->get_result_as_string( ) ).

    chk_res_dos = result->get_ou_chk_res_dos( ).  "each org unit has it's own result
    IF chk_res_dos IS INITIAL. RETURN. ENDIF.
    chk_res_do = chk_res_dos[ 1 ].

    WRITE /.
    WRITE chk_res_do->has_critical( ).  "in this case the workflow has to be stopped
    WRITE /.
    WRITE chk_res_do->has_warning( ).
    WRITE /.
    WRITE chk_res_do->has_error( ). "in this case the workflow has to wait unit manually restart
    WRITE /.
    WRITE chk_res_do->get_org_unit( ).
    WRITE /.
    WRITE chk_res_do->get_status_id( ).
  CATCH  /aeb/cx_cmp_pb_sync_chk_sc INTO exc.
    CASE exc->textid.
      WHEN /aeb/cx_cmp_pb_sync_chk_sc=>cx_bo_not_existing.
"           do error handling
      WHEN  /aeb/cx_cmp_pb_sync_chk_sc=>cx_bo_not_lockable.
"           do error handling
    ENDCASE.
    WRITE exc->/aeb/if_01_cx_message~get_msg_as_str( ).
  CATCH /aeb/cx_01_pb_missing_parm_sc INTO parm_exc.
"           do error handling
    WRITE parm_exc->get_text( ).
ENDTRY.
```

In every case of an check we call a BADI to handle the result. In case of the check out of the workflow there should not be a todo. But the next step, after a workflow is stopped because of a critical result, the business partner would be released from the Compliance Monitor and this result can be handled via the following BADIs. 

- /AEB/CMP_BUS_PRT_02 (Suspicious found)
- /AEB/CMP_BUS_PRT_03 (Nothing suspicious)
- /AEB/CMP_BUS_PRT_07 (Suspicious became good)  
  All BADIs have 3 parameters:
- im_entry_mo, which could be used to change the mdg_chg_req_no
- im_check_context, which could be used to indentify where you called from (Monitor, SyncCheck ....)
- im_ous_chk_res_do, the check results of all org units, which could be used to compare different results

```text Implementation of /AEB/CMP_BUS_PRT_07 & /AEB/CMP_BUS_PRT_03
IF im_check_context->get_check_context( ) <> im_check_context->c_chk_context_sync "Sync context is set if called from workflow
     and NOT im_entry_mo->get_mdg_chg_req_no( ) is initial.
*     call the workflow and release it 
*     if release was possible, clear the chg req no
      im_entry_mo->set_mdg_chg_req_no( ). "clear MDG chg req no, if object is not more suspicious
ENDIF.
```

```text Implementation of /AEB/CMP_BUS_PRT_02
if im_entry_mo->get_status_id( ) = 'J'. "Status forbidden, this was a real match
* 	complete the workflow
		im_entry_mo->set_mdg_chg_req_no( ).
endif.
```