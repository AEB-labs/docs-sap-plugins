---
title: Trigger asynchronous checks
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
## Trigger asynchronous checks

To trigger asynchronous checks, use the class /AEB/CL_CMP_PB_CR_ASYNC_CHK_BC and the according method:

| Business Object          | Method              |
| :----------------------- | :------------------ |
| Accounting document      | CR_AD_ASYNC_CHECK   |
| Applicant                | CR_AP_ASYNC_CHECK   |
| Bank                     | CR_BNK_ASYNC_CHECK  |
| Business Partner         | CR_BP_ASYNC_CHECK   |
| CRM Business Partner     | CR_CBP_ASYNC_CHECK  |
| CRM Business Transaction | CR_CBT_ASYNC_CHECK  |
| Customer                 | CR_CUS_ASYNC_CHECK  |
| Delivery                 | CR_DLV_ASYNC_CHECK  |
| Employee                 | CR_EM_ASYNC_CHECK   |
| Material Document        | CR_MD_ASYNC_CHECK   |
| Purchasing Document      | CR_PD_ASYNC_CHECK   |
| Sales Document           | CR_SDOC_ASYNC_CHECK |
| Service Notification     | CR_SN_ASYNC_CHECK   |
| Service Order            | CR_SO_ASYNC_CHECK   |
| Vendor                   | CR_VDR_ASYNC_CHECK  |

```text Trigger check of a business partner asynchronously
REPORT z_cr_async_check.
PARAMETERS:
   prt_no TYPE bu_partner.
DATA:
  async_chk TYPE REF TO /aeb/cl_cmp_pb_cr_async_chk_bc.

async_chk = /aeb/cl_cmp_pb_cr_async_chk_bc=>new( ).
TRY.
    async_chk->cr_bp_async_check( prt_no ).
  CATCH /aeb/cx_01_pb_missing_parm_sc.
*   do error handling
ENDTRY.
```