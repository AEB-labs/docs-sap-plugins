---
title: BAdI definitions
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
## BAdIs to determine whether an object has changed

These BAdIs read the change documents from SAP® based on the setting of the O&P data export applications to overwrite whether a given object has changed.

| Type of configurable material                  | BAdI name            |
| :--------------------------------------------- | :------------------- |
| Configurable material from a bill of materials | /AEB/WU_PLCMAT_OU_01 |
| Configurable material from an order            | /AEB/WU_SOCMAT_OU_01 |

### Configurable material from a bill of materials

BAdI method: /AEB/WU_PLCMAT_OU_01 - is_conf_mat_to_export  
Parameters:

> IM_MAST (connection material - bill of materials)  
> IM_CSTMAT (output on initial material when exploding a BOM)  
> IM_STPOX_LIST (list of BOM items (extended for list displays))  
> IM_ORG_UNIT (organizational unit)  
> CH_IS_TO_EXPORT (value “X” in the event of export, “-” if not)

### Configurable material from an order

BAdI method: /AEB/WU_SOCMAT_OU_01 - is_so_conf_mat_to_export  
Parameters:

> IM_VBAK (Sales document: header data)  
> IM_VBAP (sales document: item data)  
> IM_CSTMAT (output on initial material when exploding a BOM)  
> IM_STPOX_LIST (list of BOM items (extended for list displays))  
> IM_ORG_UNIT (organizational unit)  
> CH_IS_TO_EXPORT (value “X” in the event of export, “-” if not)

## BAdIs to change object data

These BAdIs modify the data of individual objects to be exported. The designated objects are transferred to the BAdI based on the settings in the O&P data export applications.

| Type of configurable material                  | BAdI                 |
| :--------------------------------------------- | :------------------- |
| Configurable material from a bill of materials | /AEB/WU_SOCMPL_CR_01 |
| Configurable material from an order            | /AEB/WU_SOCMAT_CR_01 |

### Configurable material from a bill of materials

BAdI method: /AEB/WU_SOCMPL_CR_01 - after_standard_filling (modify the data to be exported)  
Parameters:

> IM_VBAK (Sales document: header data)  
> IM_VBAP (sales document: header data)  
> IM_CSTMAT (output on initial material when exploding a BOM)  
> IM_STPOX_LIST (list of BOM items (extended for list displays))  
> IM_ORG_UNIT (organizational unit)  
> CH_PARTS_LIST_DO (BOM header)

### Configurable material from an order

BAdI method: /AEB/WU_SOCMAT_CR_01 - after_standard_filling (modify the data to be exported)  
Parameters:

> IM_VBAK (Sales document: header data)  
> IM_VBAP (sales document: item data)  
> IM_CSTMAT (output on initial material when exploding a BOM)  
> IM_ORG_UNIT (organizational unit)  
> CH_MATERIAL_DO (material)