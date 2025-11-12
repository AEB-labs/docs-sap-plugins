---
title: Disable Export Control checks (before data collection)
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
      slug: change-is-object-to-check-after-data-collection
      title: Disable Export control checks after data collection
---
In order to adapt the logic whether or not to trigger an export control check, you can implement a BAdI for the according business object. Use the method `is_object_to_check` in the following BAdIs:

| Business object          | BAdI                    |
| :----------------------- | :---------------------- |
| Delivery                 | /AEB/CMP\_EC\_DLV\_01   |
| Purchase document        | /AEB/CMP\_EC\_PD\_01    |
| Sales document           | /AEB/CMP\_EC\_ORDER\_04 |
| Service order            | /AEB/CMP\_EC\_SO\_01    |
| Service transaction (S4) | /AE1/CMP\_EC\_ST\_01    |

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
