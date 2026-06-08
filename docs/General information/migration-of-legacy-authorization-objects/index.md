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
      title: 'Keep authorization objects (option A) '
      type: basic
    - slug: perform-a-manual-migration
      title: Manual transfer of authorization objects (option B)
      type: basic
    - slug: switch-to-new-authorization-objects
      title: Switch to new authorization objects (option C)
      type: basic
---
<br />

In the past, AEB provided authorization objects in the Z namespace, as there was no possibility for using other namespaces. Depending on when and how you first installed AEB components in your SAP systems, you're using these Z-objects in your authorization roles. In November 2025, AEB was informed about difficulties in SAP S/4HANA migrations related to these authorization objects which could result in their deletion. This is critical, because the maintained authorization roles will become invalid.

Your SAP system is affected by this change if you have authorization objects in the system starting with ZAEB*.  (Check via transaction SU21).  The difficulties mentioned above occur when upgrading from ERP ECC to S/4HANA or when  upgrading your S/4HANA system to a new release,e.g. from 2020 to 2025. 

You have the following options to proceed:

* Keep the existing authorization objects from AEB and prevent any changes to the existing roles.  (option A and B) 
* Switch to the new authorization objects and change the existing roles (opion C) 

<Anchor label="Option A" target="_blank" href="https://sap-plugins.docs.developers.aeb.com/docs/use-a-report-for-the-migration">Option A</Anchor>  Execute a report to analyze which of your roles are concerned. Contact the AEB support with that information to transfer the concerned Z-authorization objects to a new package. 

or

<Anchor label="Option B" target="_blank" href="https://sap-plugins.docs.developers.aeb.com/docs/perform-a-manual-migration">Option B</Anchor>  Perform a manual transfer of the authorization objects. 

or

<Anchor label="Option C" target="_blank" href="https://sap-plugins.docs.developers.aeb.com/docs/switch-to-new-authorization-objects">Option C</Anchor>   Switch to new authorization objects and change the existing roles. Delete the old authorization objects.

Select your next step accordingly in the following "What's next" - section:

<br />
