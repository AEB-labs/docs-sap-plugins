---
title: Disable screening checks
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
In order to adapt the logic whether or not to trigger a screening check, you can implement  a BAdI for each business object . Use the method `is_object_to_check` in the following BAdIs: 

| Business object          | BAdI                    |
| :----------------------- | :---------------------- |
| Account document         | /AEB/CMP\_ACC\_DC\_01   |
| Applicant                | /AEB/CMP\_APPLICAN\_01  |
| Bank                     | /AEB/CMP\_BANK\_01      |
| Business partner         | /AEB/CMP\_BUS\_PRT\_01  |
| Customer                 | /AEB/CMP\_CUSTOMER\_01  |
| Delivery                 | /AEB/CMP\_DLV\_DC\_01   |
| Employee                 | /AEB/CMP\_EMPLOYEE\_01  |
| Material document        | /AEB/CMP\_MAT\_DC\_01   |
| Purchase document        | /AEB/CMP\_PURCH\_DC\_01 |
| Payment                  | /AEB/CMP\_PIP\_01       |
| Sales document           | /AEB/CMP\_SALES\_DC\_01 |
| Service notification     | /AEB/CMP\_SERV\_NOT\_01 |
| Service order            | /AEB/CMP\_SERV\_ORD\_01 |
| Vendor                   | /AEB/CMP\_VENDOR\_01    |
| Service transaction (S4) | /AE1/CMP\_SERV\_TRA\_01 |

The BAdI `is_object_to_check` can be implemented in two different ways:

1. **skip-check logic** – to prevent a re-check for certain changes that do not affect the check result (for example, technical updates or automated rescheduling).  
2. **exclude-from-check logic** – to define business objects that are generally not relevant for compliance or export control checks (for example, specific document types or processes).

> ❗️ Consistency required between Screening and Export Controls
>
> Because the overall result is derived from both Compliance Screening and Export Controls, you must take care when mixing the **skip-check logic** across modules:\
> if you skip only one module (e.g. Export Controls) while leaving the other (Screening) active, the composite evaluation may become inconsistent.\
> In that scenario, a previously blocked object (because of an Export Controls restriction) might appear unblocked simply because only Screening executes and returns non-critical.
>
> **Therefore:**\
> If you implement the BAdI `is_object_to_check` **with the intention of skipping checks** (using the *skip-check logic*), you **must** ensure that both modules — *Compliance Screening* and *Export Controls* — are either skipped **together** or executed **together**.\
> Mixing skip behavior between modules will lead to inconsistent overall results and may incorrectly unblock business objects that should remain blocked.
>
> In contrast, when you use the same BAdI in the sense of an *exclude-from-check logic*, it is perfectly valid to define independent relevance for *Compliance Screening* and *Export Controls*, depending on your specific business process requirements.

Here is an example that does exclude a certain sales order type from the check:

```Text Do not check certain sales order types
If im_vbak-vbtyp eq 'XYZ'
   CH_IS_OBJECT_TO_CHECK = '-'.
else
  CH_IS_OBJECT_TO_CHECK ='X'.
endif.
```
