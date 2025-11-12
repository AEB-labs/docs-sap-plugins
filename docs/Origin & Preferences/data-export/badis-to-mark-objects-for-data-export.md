---
title: BAdIs to mark objects for data export
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
      slug: badis-to-change-object-data
      title: BAdIs to change object data
---
These BAdIs mark individual objects to be exported. The designated objects are transferred to the BAdI based on the settings in the O\&P data export applications.

| Business object   | BAdI name            |
| :---------------- | :------------------- |
| Customer          | /AEB/WU\_CUS\_OU\_01 |
| Vendor            | /AEB/WU\_VDR\_OU\_01 |
| Material          | /AEB/WU\_MAT\_OU\_01 |
| Goods receipt     | /AEB/WU\_GR\_OU\_01  |
| Incoming invoice  | /AEB/WU\_II\_OU\_01  |
| Bill of materials | /AEB/WU\_PL\_OU\_01  |
| Purchase order    | /AEB/WU\_PO\_OU\_01  |
| Sales order       | /AEB/WU\_SO\_OU\_01  |
| Invoice           | /AEB/WU\_INV\_OU\_01 |

# Customer

BAdI method: /AEB/WU\_CUS\_OU\_01 - is\_customer\_to\_export\
Parameters:

> IM\_KNA1 (customer master (general part))\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_IS\_TO\_EXPORT (“X” for true, “-” for false)

# Vendor

BAdI method: /AEB/WU\_VDR\_OU\_01 - is\_vendor\_to\_export\
Parameters:

> IM\_LFA1 (supplier master (general part))\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_IS\_TO\_EXPORT (“X” for true, “-” for false)

# Material

BAdI method: /AEB/WU\_MAT\_OU\_01 - is\_material\_to\_export\
Parameters:

> IM\_MARA (general material data)\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_IS\_TO\_EXPORT (“X” for true, “-” for false)

# Goods receipt

BAdI method: /AEB/WU\_GR\_OU\_01 - is\_goods\_receipt\_to\_export\
Parameters:

> IM\_MKPF (material document header)\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_IS\_TO\_EXPORT (“X” for true, “-” for false)

# Incoming invoice

BAdI method: /AEB/WU\_II\_OU\_01 - is\_incoming\_invoice\_to\_export\
Parameters:

> IM\_CDHDR (change document header)\
> IM\_RBKP (incoming invoice document header)\
> CH\_HAS\_CHANGED (“X” for true, “-” for false)

# Bill of materials

BAdI method: /AEB/WU\_PL\_OU\_01 - is\_parts\_list\_to\_export\
Parameters:

> IM\_STKO (BOM header)\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_IS\_TO\_EXPORT (“X” for true, “-” for false)

# Purchase order

BAdI method: /AEB/WU\_PO\_OU\_01 - is\_purchase\_order\_to\_export\
Parameters:

> IM\_EKKO (purchasing document header)\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_IS\_TO\_EXPORT (“X” for true, “-” for false)

# Sales order

BAdI method: /AEB/WU\_SO\_OU\_01 - is\_sales\_order\_to\_export\
Parameters:

> IM\_VBAK (Sales document: header data)\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_IS\_TO\_EXPORT (“X” for true, “-” for false)
