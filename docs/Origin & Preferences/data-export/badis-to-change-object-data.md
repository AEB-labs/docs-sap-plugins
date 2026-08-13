---
title: BAdIs to change object data
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
These BAdIs modify the data of individual objects to be exported. The designated objects are transferred to the BAdI based on the settings in the O\&P data export applications.

| Business object   | BAdI name         |
| :---------------- | :---------------- |
| Customer          | /AEB/WU_CUS_CR_01 |
| Vendor            | /AEB/WU_VDR_CR_01 |
| Material          | /AEB/WU_MAT_CR_01 |
| Goods receipt     | /AEB/WU_GR_CR_01  |
| Incoming invoice  | /AEB/WU_II_CR_01  |
| Bill of materials | /AEB/WU_PL_CR_01  |
| Purchase order    | /AEB/WU_PO_CR_01  |
| Sales order       | /AEB/WU_SO_CR_01  |
| Invoice           | /AEB/WU_INV_CR_01 |

# Customer

BAdI method: /AEB/WU_CUS_CR_01 - before_standard_filling (modify the parameters for how the data is populated by default)<br />Parameters:

> IM_KNA1 (customer master (general part))<br />IM_ORG_UNIT (organizational unit)<br />IM_KNVKS (list of contact persons in customer master)<br />EX_KNVK_ADRND (business address)<br />EX_SPRAS (language key)

BAdI method: /AEB/WU_CUS_CR_01 - after_standard_filling (modify the data to be exported)<br />Parameters:

> IM_KNA1 (customer master (general part))<br />IM_ORG_UNIT (organizational unit)<br />IM_KNVKS (list of contact persons in customer master)<br />CH_ADDRESS_DO (address)

# Vendor

BAdI method: /AEB/WU_VDR_CR_01 - before_standard_filling (modify the parameters for how the data is populated by default)<br />Parameters:

> IM_LFA1 (supplier master (general part))<br />IM_ORG_UNIT (organizational unit)<br />IM_LFM1S (list of purchasing organization data in supplier master)<br />EX_EKORG (purchasing organization)

BAdI method: /AEB/WU_VDR_CR_01 - after_standard_filling (modify the data to be exported)<br />Parameters:

> IM_LFA1 (supplier master (general part))<br />IM_LFM1 (purchasing organization data in supplier master)<br />IM_ORG_UNIT (organizational unit)<br />CH_ADDRESS_DO (address)

# Material

BAdI method: /AEB/WU_MAT_CR_01 - before_standard_filling (modify the parameters for how the data is populated by default)<br />Parameters:

> IM_MARA (general material data)<br />IM_MARCS (list of factory data for material)<br />IM_MBEWS (list of material evaluation)<br />IM_ORG_UNIT (organizational unit)<br />EX_MARC_WERKS (factory)<br />EX_MBEW_BWKEY (valuation area)<br />EX_MBEW_BWTAR (valuation type)

BAdI method: /AEB/WU_MAT_CR_01 - after_standard_filling (modify the data to be exported)<br />Parameters:

> IM_MARA (general material data)<br />IM_MARC (factory data for material)<br />IM_MBEW (material evaluation)<br />IM_ORG_UNIT (organizational unit)<br />CH_MATERIAL_DO (material)

# Goods receipt

BAdI method: /AEB/WU_GR_CR_01 - after_standard_filling (modify the data to be exported)<br />Parameters:

> IM_MKPF (material document header)<br />IM_MSEGS (list of document segment for material)<br />IM_ORG_UNIT (organizational unit)<br />CH_GOODS_RECEIPT_DO (goods receipt)

# Incoming invoice

BAdI method: /AEB/WU_II_CR_01 - after_standard_filling (modify the data to be exported)<br />Parameters:

> IM_RBKP (incoming invoice document header in customer master)<br />IM_RSEGS (list of document items in incoming invoice)<br />IM_ORG_UNIT (organizational unit)<br />CH_GOODS_RECEIPT_DO (goods receipt)

# Bill of materials

BAdI method: /AEB/WU_PL_CR_01 - before_standard_filling (modify the parameters for how the data is populated by default)<br />Parameters:

> IM_STKO (BOM header)<br />IM_STPOS (list of bill of materials items)<br />IM_ORG_UNIT (organizational unit)<br />EX_WERKS (factory)<br />EX_MAST_STLAN (use of bill of materials)<br />EX_MBEW_BWKEY (valuation area)<br />EX_MBEW_BWTAR (valuation type)

BAdI method: /AEB/WU_PL_CR_01 - after_standard_filling (modify the data to be exported)<br />Parameters:

> IM_STKO (BOM header)<br />IM_STPOS (list of bill of materials items)<br />IM_ORG_UNIT (organizational unit)<br />CH_PARTS_LIST_DO (BOM header)

# Purchase order

BAdI method: /AEB/WU_PO_CR_01 - after_standard_filling (modify the data to be exported)<br />Parameters:

> IM_EKKO (purchasing document header)<br />IM_EKPOS (list of purchasing document items)<br />IM_ORG_UNIT (organizational unit)<br />IM_EXP_SESSION_ID (session ID)<br />CH_PURCHASE_ORDER_DO (purchase order)

# Sales order

BAdI method: /AEB/WU_SO_CR_01 - after_standard_filling (modify the data to be exported)<br />Parameters:

> IM_VBAK (Sales document: header data)<br />IM_VBAPS (list of sales document: item data)<br />IM_ORG_UNIT (organizational unit)<br />IM_EXP_SESSION_ID (session ID)<br />CH_SALES_ORDER_DO (sales order)

# Invoice

BAdI methode: /AEB/WU_INV_CR_01 - after_standard_filling (modify the data to be exported)<br />Parameters:

> IM_VBRK (Invoice: header data)<br />IM_VBRPS (list of invoice: item data)<br />IM_ORG_UNIT (organizational unit)<br />IM_EXP_SESSION_ID (session ID)<br />CH_SALES_ORDER_DO (sales order)
