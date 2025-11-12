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
All functions are located in the function group /AEB/WU_PB_CREATE_EXP_DATA.

- /AEB/WU_PB_CR_GOODS_RECEIPT
- /AEB/WU_PB_CR_MATERIAL
- /AEB/WU_PB_CR_PARTS_LIST
- /AEB/WU_PB_CR_SALES_ORDER

# /AEB/WU_PB_CR_GOODS_RECEIPT

This function module offers the function of creating and transferring goods receipt data.

Import parameters:

> IM_GOODS_RECEIPT_DO (goods receipt data)

Exceptions:

> /AEB/CX_WU_PB_EXPORT_SC (Exception during data transfer)

# /AEB/WU_PB_CR_MATERIAL

This function module offers the function of creating and transferring material data.

Import parameters:

> IM_MATERIAL_DO (material data)

Exceptions:

> /AEB/CX_WU_PB_EXPORT_SC (Exception during data transfer)

# /AEB/WU_PB_CR_PARTS_LIST

This function module offers the function of creating and transferring bills of material.

Import parameters:

> IM_PARTS_LIST_DO (BOM header)

Exceptions:

> /AEB/CX_WU_PB_EXPORT_SC (Exception during data transfer)

# /AEB/WU_PB_CR_SALES_ORDER

This function module offers the function of creating und transferring order data.

Import parameters:

> IM_SALES_ORDER_DO (order data)

Exceptions:

> /AEB/CX_WU_PB_EXPORT_SC (Exception during data transfer)