---
title: BAdIs to determine whether an object has changed
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
  pages:
    - type: basic
      slug: badis-to-mark-objects-for-data-export
      title: BAdIs to mark objects for data export
---
These BAdIs read the change documents from SAP® based on the setting of the O&P data export applications to overwrite whether a given object has changed.

| Business object   | BAdI name         |
| :---------------- | :---------------- |
| Customer          | /AEB/WU_CUS_CC_01 |
| Vendor            | /AEB/WU_VDR_CC_01 |
| Material          | /AEB/WU_MAT_CC_01 |
| Goods receipt     | /AEB/WU_GR_CC_01  |
| Incoming invoice  | /AEB/WU_II_CC_01  |
| Bill of materials | /AEB/WU_PL_CC_01  |
| Purchase order    | /AEB/WU_PO_CC_01  |
| Sales order       | /AEB/WU_SO_CC_01  |
| Invoice           | /AEB/WU_INV_CC_01 |

# Customer

BAdI method: /AEB/WU_CUS_CC_01 - has_changed  
Parameters:

> IM_CDHDR (change document header)  
> IM_KNA1 (customer master (general part))  
> IM_DATE (decisive date (date of the last export or date specified in the export))  
> CH_HAS_CHANGED (“X” for true, “-” for false)

# Vendor

BAdI method: /AEB/WU_VDR_CC_01 - has_changed  
Parameters:

> IM_CDHDR (change document header)  
> IM_LFA1 (supplier master (general part))  
> IM_DATE (decisive date (date of the last export or date specified in the export))  
> CH_HAS_CHANGED (“X” for true, “-” for false)

# Material

BAdI method: /AEB/WU_MAT_CC_01 - has_changed  
Parameters:

> IM_CDHDR (change document header)  
> IM_MARA (general material data)  
> IM_DATE (decisive date (date of the last export or date specified in the export))  
> CH_HAS_CHANGED (“X” for true, “-” for false)

# Goods receipt

BAdI method: /AEB/WU_GR_CC_01 - has_changed  
Parameters:

> IM_MKPF (material document header)  
> IM_DATE (decisive date (date of the last export or date specified in the export))  
> CH_HAS_CHANGED (“X” for true, “-” for false)

# Incoming invoice

BAdI method: /AEB/WU_II_CC_01 - has_changed  
Parameters:

> IM_CDHDR (change document header)  
> IM_RBKP (incoming invoice document header)  
> IM_DATE (decisive date (date of the last export or date specified in the export))  
> CH_HAS_CHANGED (“X” for true, “-” for false)

# Bill of materials

BAdI method: /AEB/WU_PL_CC_01 - has_changed  
Parameters:

> IM_CDHDR (change document header)  
> IM_STKO (BOM header)  
> IM_DATE (decisive date (date of the last export or date specified in the export))  
> CH_HAS_CHANGED (“X” for true, “-” for false)

# Purchase order

BAdI method: /AEB/WU_PO_CC_01 - has_changed  
Parameters:

> IM_CDHDR (change document header)  
> IM_EKKO (purchasing document header)  
> IM_DATE (decisive date (date of the last export or date specified in the export))  
> CH_HAS_CHANGED (“X” for true, “-” for false)

# Sales order

BAdI method: /AEB/WU_SO_CC_01 - has_changed  
Parameters:

> IM_CDHDR (change document header)  
> IM_VBAK (Sales document: header data)  
> IM_DATE (decisive date (date of the last export or date specified in the export))  
> CH_HAS_CHANGED (“X” for true, “-” for false)