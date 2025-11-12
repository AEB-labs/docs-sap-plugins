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
These BAdIs mark individual objects to be exported. The designated objects are transferred to the BAdI based on the settings in the O&P data export applications.

| Business object   | BAdI name         |
| :---------------- | :---------------- |
| Customer          | /AEB/WU_CUS_OU_01 |
| Vendor            | /AEB/WU_VDR_OU_01 |
| Material          | /AEB/WU_MAT_OU_01 |
| Goods receipt     | /AEB/WU_GR_OU_01  |
| Incoming invoice  | /AEB/WU_II_OU_01  |
| Bill of materials | /AEB/WU_PL_OU_01  |
| Purchase order    | /AEB/WU_PO_OU_01  |
| Sales order       | /AEB/WU_SO_OU_01  |
| Invoice           | /AEB/WU_INV_OU_01 |

# Customer

BAdI method: /AEB/WU_CUS_OU_01 - is_customer_to_export  
Parameters:

> IM_KNA1 (customer master (general part))  
> IM_ORG_UNIT (organizational unit)  
> CH_IS_TO_EXPORT (“X” for true, “-” for false)

# Vendor

BAdI method: /AEB/WU_VDR_OU_01 - is_vendor_to_export  
Parameters:

> IM_LFA1 (supplier master (general part))  
> IM_ORG_UNIT (organizational unit)  
> CH_IS_TO_EXPORT (“X” for true, “-” for false)

# Material

BAdI method: /AEB/WU_MAT_OU_01 - is_material_to_export  
Parameters:

> IM_MARA (general material data)  
> IM_ORG_UNIT (organizational unit)  
> CH_IS_TO_EXPORT (“X” for true, “-” for false)

# Goods receipt

BAdI method: /AEB/WU_GR_OU_01 - is_goods_receipt_to_export  
Parameters:

> IM_MKPF (material document header)  
> IM_ORG_UNIT (organizational unit)  
> CH_IS_TO_EXPORT (“X” for true, “-” for false)

# Incoming invoice

BAdI method: /AEB/WU_II_OU_01 - is_incoming_invoice_to_export  
Parameters:

> IM_CDHDR (change document header)  
> IM_RBKP (incoming invoice document header)  
> CH_HAS_CHANGED (“X” for true, “-” for false)

# Bill of materials

BAdI method: /AEB/WU_PL_OU_01 - is_parts_list_to_export  
Parameters:

> IM_STKO (BOM header)  
> IM_ORG_UNIT (organizational unit)  
> CH_IS_TO_EXPORT (“X” for true, “-” for false)

# Purchase order

BAdI method: /AEB/WU_PO_OU_01 - is_purchase_order_to_export  
Parameters:

> IM_EKKO (purchasing document header)  
> IM_ORG_UNIT (organizational unit)  
> CH_IS_TO_EXPORT (“X” for true, “-” for false)

# Sales order

BAdI method: /AEB/WU_SO_OU_01 - is_sales_order_to_export  
Parameters:

> IM_VBAK (Sales document: header data)  
> IM_ORG_UNIT (organizational unit)  
> CH_IS_TO_EXPORT (“X” for true, “-” for false)