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

| Use for                                                                       | Name of the function module or BAdI  |
| :---------------------------------------------------------------------------- | :----------------------------------- |
| Delete the Export Controls result                                             | /AEB/CMP\_DELETE\_EC\_FOR\_VBELN     |
| Do a custom EC check, returning the total result                              | /AEB/CMP\_PB\_EC\_BO\_CHK\_COMPLEX   |
| Do a custom EC check, and get the result (critical or not) back.              | /AEB/CMP\_PB\_EC\_BO\_CHK\_SIMPLE    |
| Delete approvals / clearings for a reference                                  | /AEB/CMP\_PB\_EC\_DELETE\_CLEA\_FOR  |
| Add additional customs data to the approval                                   | /AEB/CMP\_PB\_EC\_ADD\_CI\_FOR\_DLV  |
| Finalize an approval and add customs data                                     | /AEB/CMP\_PB\_EC\_ATL\_FIN\_FOR\_DLV |
| Finalize an approval                                                          | /AEB/CMP\_PB\_EC\_FIN\_FOR\_DLV      |
| Adapt the function to open a material record from the Export Controls Cockpit | /AEB/CMP\_EC\_CP\_01  (BAdI )        |
