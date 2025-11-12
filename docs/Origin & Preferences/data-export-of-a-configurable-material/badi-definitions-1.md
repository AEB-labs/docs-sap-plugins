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

These BAdIs read the change documents from SAP® based on the setting of the O\&P data export applications to overwrite whether a given object has changed.

| Type of configurable material                  | BAdI name               |
| :--------------------------------------------- | :---------------------- |
| Configurable material from a bill of materials | /AEB/WU\_PLCMAT\_OU\_01 |
| Configurable material from an order            | /AEB/WU\_SOCMAT\_OU\_01 |

### Configurable material from a bill of materials

BAdI method: /AEB/WU\_PLCMAT\_OU\_01 - is\_conf\_mat\_to\_export\
Parameters:

> IM\_MAST (connection material - bill of materials)\
> IM\_CSTMAT (output on initial material when exploding a BOM)\
> IM\_STPOX\_LIST (list of BOM items (extended for list displays))\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_IS\_TO\_EXPORT (value “X” in the event of export, “-” if not)

### Configurable material from an order

BAdI method: /AEB/WU\_SOCMAT\_OU\_01 - is\_so\_conf\_mat\_to\_export\
Parameters:

> IM\_VBAK (Sales document: header data)\
> IM\_VBAP (sales document: item data)\
> IM\_CSTMAT (output on initial material when exploding a BOM)\
> IM\_STPOX\_LIST (list of BOM items (extended for list displays))\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_IS\_TO\_EXPORT (value “X” in the event of export, “-” if not)

## BAdIs to change object data

These BAdIs modify the data of individual objects to be exported. The designated objects are transferred to the BAdI based on the settings in the O\&P data export applications.

| Type of configurable material                  | BAdI                    |
| :--------------------------------------------- | :---------------------- |
| Configurable material from a bill of materials | /AEB/WU\_SOCMPL\_CR\_01 |
| Configurable material from an order            | /AEB/WU\_SOCMAT\_CR\_01 |

### Configurable material from a bill of materials

BAdI method: /AEB/WU\_SOCMPL\_CR\_01 - after\_standard\_filling (modify the data to be exported)\
Parameters:

> IM\_VBAK (Sales document: header data)\
> IM\_VBAP (sales document: header data)\
> IM\_CSTMAT (output on initial material when exploding a BOM)\
> IM\_STPOX\_LIST (list of BOM items (extended for list displays))\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_PARTS\_LIST\_DO (BOM header)

### Configurable material from an order

BAdI method: /AEB/WU\_SOCMAT\_CR\_01 - after\_standard\_filling (modify the data to be exported)\
Parameters:

> IM\_VBAK (Sales document: header data)\
> IM\_VBAP (sales document: item data)\
> IM\_CSTMAT (output on initial material when exploding a BOM)\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_MATERIAL\_DO (material)
