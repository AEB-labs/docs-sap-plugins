---
title: Migration of legacy authorization objects
deprecated: false
hidden: true
metadata:
  robots: index
next:
  pages:
    - slug: use-a-report-for-the-migration
      title: Use a report for the migration
      type: basic
    - slug: perform-a-manual-migration
      title: Perform a manual migration
      type: basic
---
<br />

In the past, AEB delivered authorization objects in the Z namespace. Depending on when and how you first installed AEB components in your SAP systems, you have installed and maintained these objects in authorization roles. In November 2025, AEB was informed about difficulties in S/4HANA migrations related to these authorization objects which could result in their deletion. This is critical, because maintained authorization roles will become invalid. You have the following options to prevent this:

1. Switch to new authorization objects

<br />

1. Manually move the objects to another package.
   The new package name must begin with 'Z'. However, do not select any of the ZAEB_*_NAMESPACELESS_OBJECTS packages. Example: ZAEB_DEPRECATED_OBJECTS.

To move the objects, first create the new package. After that, you can use the Repository Information System. Open the Object Navigator (SE80) and select the Repository Information System. Open "Other objects -> Authorization Objects". Use the select option of the field "Authorization Object" to enter the following list of objects. Use the function "Upload from clipboard" to conveniently insert the objects. Execute the selection. On the result list, select all entries and perform the menu function "Auth. objects -> Reassign". Enter the new package name and execute the function.

3. Switch to the new authorization objects. Each Z authorization object has been replaced by an authorization object in the /AEB/ namespace. In order for these to be used, the legacy Z authorization objects must be deleted from your system. All uses in roles must be removed before deletion, otherwise the roles will become invalid.
