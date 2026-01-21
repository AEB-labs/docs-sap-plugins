---
title: Transfer of legacy authorization objects
deprecated: false
hidden: false
metadata:
  robots: index
next:
  description: Choose from the following options
  pages:
    - slug: use-a-report-for-the-migration
      title: Check used authorization objects
      type: basic
    - slug: perform-a-manual-migration
      title: Perform manual transfer of authorization objects
      type: basic
    - slug: switch-to-new-authorization-objects
      title: Switch to new authorization objects
      type: basic
---
<br />

In the past, AEB provided authorization objects in the Z namespace, as there was no possibility for using other namespaces. Depending on when and how you first installed AEB components in your SAP systems, you're using these Z-objects in your authorization roles. In November 2025, AEB was informed about difficulties in SAP S/4HANA migrations related to these authorization objects which could result in their deletion. This is critical, because the maintained authorization roles will become invalid. 

Your SAP system is affected by this change in the following cases:

* You have authorization objects in the system starting with ZAEB*.    (check via transaction SU21)
* You are upgrading from ERP ECC to S/4HANA or upgrading your S/4HANA system to a new release

You have the following options to prevent unwanted changes to your authorization roles:

1. Execute a report to analyze if your roles are concerned. Contact the AEB support with that information to transfer the concerned Z-authorization objects to a new package.
2. Perform a manual transfer of the authorization objects
3. Switch to new authorization objects

Select your next step accordingly in the following section.

<br />
