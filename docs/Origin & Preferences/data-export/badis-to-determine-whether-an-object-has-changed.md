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
These BAdIs read the change documents from SAP® based on the setting of the O\&P data export applications to overwrite whether a given object has changed.

| Business object   | BAdI name            |
| :---------------- | :------------------- |
| Customer          | /AEB/WU\_CUS\_CC\_01 |
| Vendor            | /AEB/WU\_VDR\_CC\_01 |
| Material          | /AEB/WU\_MAT\_CC\_01 |
| Goods receipt     | /AEB/WU\_GR\_CC\_01  |
| Incoming invoice  | /AEB/WU\_II\_CC\_01  |
| Bill of materials | /AEB/WU\_PL\_CC\_01  |
| Purchase order    | /AEB/WU\_PO\_CC\_01  |
| Sales order       | /AEB/WU\_SO\_CC\_01  |
| Invoice           | /AEB/WU\_INV\_CC\_01 |

# Customer

BAdI method: /AEB/WU\_CUS\_CC\_01 - has\_changed\
Parameters:

> IM\_CDHDR (change document header)\
> IM\_KNA1 (customer master (general part))\
> IM\_DATE (decisive date (date of the last export or date specified in the export))\
> CH\_HAS\_CHANGED (“X” for true, “-” for false)

# Vendor

BAdI method: /AEB/WU\_VDR\_CC\_01 - has\_changed\
Parameters:

> IM\_CDHDR (change document header)\
> IM\_LFA1 (supplier master (general part))\
> IM\_DATE (decisive date (date of the last export or date specified in the export))\
> CH\_HAS\_CHANGED (“X” for true, “-” for false)

# Material

BAdI method: /AEB/WU\_MAT\_CC\_01 - has\_changed\
Parameters:

> IM\_CDHDR (change document header)\
> IM\_MARA (general material data)\
> IM\_DATE (decisive date (date of the last export or date specified in the export))\
> CH\_HAS\_CHANGED (“X” for true, “-” for false)

# Goods receipt

BAdI method: /AEB/WU\_GR\_CC\_01 - has\_changed\
Parameters:

> IM\_MKPF (material document header)\
> IM\_DATE (decisive date (date of the last export or date specified in the export))\
> CH\_HAS\_CHANGED (“X” for true, “-” for false)

# Incoming invoice

BAdI method: /AEB/WU\_II\_CC\_01 - has\_changed\
Parameters:

> IM\_CDHDR (change document header)\
> IM\_RBKP (incoming invoice document header)\
> IM\_DATE (decisive date (date of the last export or date specified in the export))\
> CH\_HAS\_CHANGED (“X” for true, “-” for false)

# Bill of materials

BAdI method: /AEB/WU\_PL\_CC\_01 - has\_changed\
Parameters:

> IM\_CDHDR (change document header)\
> IM\_STKO (BOM header)\
> IM\_DATE (decisive date (date of the last export or date specified in the export))\
> CH\_HAS\_CHANGED (“X” for true, “-” for false)

# Purchase order

BAdI method: /AEB/WU\_PO\_CC\_01 - has\_changed\
Parameters:

> IM\_CDHDR (change document header)\
> IM\_EKKO (purchasing document header)\
> IM\_DATE (decisive date (date of the last export or date specified in the export))\
> CH\_HAS\_CHANGED (“X” for true, “-” for false)

# Sales order

BAdI method: /AEB/WU\_SO\_CC\_01 - has\_changed\
Parameters:

> IM\_CDHDR (change document header)\
> IM\_VBAK (Sales document: header data)\
> IM\_DATE (decisive date (date of the last export or date specified in the export))\
> CH\_HAS\_CHANGED (“X” for true, “-” for false)
