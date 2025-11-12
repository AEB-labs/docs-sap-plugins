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

To trigger asynchronous checks, use the class /AEB/CL\_CMP\_PB\_CR\_ASYNC\_CHK\_BC and the according method:

| Business Object          | Method                 |
| :----------------------- | :--------------------- |
| Accounting document      | CR\_AD\_ASYNC\_CHECK   |
| Applicant                | CR\_AP\_ASYNC\_CHECK   |
| Bank                     | CR\_BNK\_ASYNC\_CHECK  |
| Business Partner         | CR\_BP\_ASYNC\_CHECK   |
| CRM Business Partner     | CR\_CBP\_ASYNC\_CHECK  |
| CRM Business Transaction | CR\_CBT\_ASYNC\_CHECK  |
| Customer                 | CR\_CUS\_ASYNC\_CHECK  |
| Delivery                 | CR\_DLV\_ASYNC\_CHECK  |
| Employee                 | CR\_EM\_ASYNC\_CHECK   |
| Material Document        | CR\_MD\_ASYNC\_CHECK   |
| Purchasing Document      | CR\_PD\_ASYNC\_CHECK   |
| Sales Document           | CR\_SDOC\_ASYNC\_CHECK |
| Service Notification     | CR\_SN\_ASYNC\_CHECK   |
| Service Order            | CR\_SO\_ASYNC\_CHECK   |
| Vendor                   | CR\_VDR\_ASYNC\_CHECK  |

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
