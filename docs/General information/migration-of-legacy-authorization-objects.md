---
title: Migration of legacy authorization objects
deprecated: false
hidden: true
metadata:
  robots: index
---
<br />

In the past, AEB delivered authorization objects in the Z namespace. Depending on when and how you first installed AEB components in your SAP systems, you maintained these objects in authorization roles. AEB was informed about difficulties in S/4HANA migrations related to these authorization objects which could result in their deletion. You have the following options to prevent this:

1. Copy the following code block into a report and execute it in your system. The output of the report you will inform you about the next steps.  
   <Accordion title="Report code">
   </Accordion>

1. Manually move the objects to another package.
   Move all objects The package name must begin with a Z, otherwise it is freely selectable. However, do not select any of the ZAEB_*_NAMESPACELESS_OBJECTS packages.
   Four objects have additionally maintained documentation, which is available as a separate entry in the package. However, they are not displayed in the SE80. The objects in the TADIR table are as follows:
   UOZAEB_AAW00
   UOZAEB_AEB00
   UOZAEB_AES00
   UOZAEB_AOU01
   This documentation has no function and should be deleted. To do this, log on to the system in English and start transaction SU21. Open the relevant authorization object (without the UO at the beginning). Now switch to change mode and click on "Create Object Documentation". On the next screen, execute the "Delete" function via the menu.

2. Switch to the new authorization objects. Each Z authorization object has been replaced by an authorization object in the /AEB/ namespace. In order for these to be used, the legacy Z authorization objects must be deleted from your system. All uses in roles must be removed before deletion, otherwise the roles will become invalid.
