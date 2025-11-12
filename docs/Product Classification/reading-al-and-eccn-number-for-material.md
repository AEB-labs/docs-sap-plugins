---
title: Read classifications and certificates of a material
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
      slug: read-classification-values
      title: Read classifications
    - type: basic
      slug: read-classifications
      title: Read classification certificates
---
The class /AEB/CL_TA_PB_SMAT_READ_V1_BC can be used to read classifications of a material.  Use the material number and the ID of the plant as parameters in the call.  

Make sure you're using a plant that is relevant for the material and that the plant was assigned to an organizational unit in the configuration of the AEB add-on.  

Keep in mind that you need to synchronize the data first by running the program /AEB/01_JE_SYNC and afterwards the program /AEB/01_JE_WORKER. Only then you can acces the classification data of a material.