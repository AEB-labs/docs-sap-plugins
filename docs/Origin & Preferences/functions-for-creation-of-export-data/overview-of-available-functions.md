---
title: Overview of available functions
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
      slug: further-badi-implementations
      title: Further BAdI definitions
---
All functions are located in the function group /AEB/WU\_PB\_CREATE\_EXP\_DATA.

* /AEB/WU\_PB\_CR\_GOODS\_RECEIPT
* /AEB/WU\_PB\_CR\_MATERIAL
* /AEB/WU\_PB\_CR\_PARTS\_LIST
* /AEB/WU\_PB\_CR\_SALES\_ORDER

# /AEB/WU\_PB\_CR\_GOODS\_RECEIPT

This function module offers the function of creating and transferring goods receipt data.

Import parameters:

> IM\_GOODS\_RECEIPT\_DO (goods receipt data)

Exceptions:

> /AEB/CX\_WU\_PB\_EXPORT\_SC (Exception during data transfer)

# /AEB/WU\_PB\_CR\_MATERIAL

This function module offers the function of creating and transferring material data.

Import parameters:

> IM\_MATERIAL\_DO (material data)

Exceptions:

> /AEB/CX\_WU\_PB\_EXPORT\_SC (Exception during data transfer)

# /AEB/WU\_PB\_CR\_PARTS\_LIST

This function module offers the function of creating and transferring bills of material.

Import parameters:

> IM\_PARTS\_LIST\_DO (BOM header)

Exceptions:

> /AEB/CX\_WU\_PB\_EXPORT\_SC (Exception during data transfer)

# /AEB/WU\_PB\_CR\_SALES\_ORDER

This function module offers the function of creating und transferring order data.

Import parameters:

> IM\_SALES\_ORDER\_DO (order data)

Exceptions:

> /AEB/CX\_WU\_PB\_EXPORT\_SC (Exception during data transfer)
