---
title: Further Export Controls functions
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
      slug: read-results-of-export-controls
      title: Read data from Export Controls and Risk Assessment
    - type: basic
      slug: further-badis
      title: Adapt creation of ELANK2 transaction
---
There are some additional functions related to export controls:

| Use for                                                                       | Name of the function module or BAdI |
| :---------------------------------------------------------------------------- | :---------------------------------- |
| Delete the Export Controls result                                             | /AEB/CMP_DELETE_EC_FOR_VBELN        |
| Do a custom EC check, returning the total result                              | /AEB/CMP_PB_EC_BO_CHK_COMPLEX       |
| Do a custom EC check, and get the result (critical or not) back.              | /AEB/CMP_PB_EC_BO_CHK_SIMPLE        |
| Delete approvals / clearings for a reference                                  | /AEB/CMP_PB_EC_DELETE_CLEA_FOR      |
| Add additional customs data to the approval                                   | /AEB/CMP_PB_EC_ADD_CI_FOR_DLV       |
| Finalize an approval and add customs data                                     | /AEB/CMP_PB_EC_ATL_FIN_FOR_DLV      |
| Finalize an approval                                                          | /AEB/CMP_PB_EC_FIN_FOR_DLV          |
| Adapt the function to open a material record from the Export Controls Cockpit | /AEB/CMP_EC_CP_01  (BAdI )          |