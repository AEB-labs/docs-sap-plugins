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

| Business object   | BAdI name            |
| :---------------- | :------------------- |
| Customer          | /AEB/WU\_CUS\_CR\_01 |
| Vendor            | /AEB/WU\_VDR\_CR\_01 |
| Material          | /AEB/WU\_MAT\_CR\_01 |
| Goods receipt     | /AEB/WU\_GR\_CR\_01  |
| Incoming invoice  | /AEB/WU\_II\_CR\_01  |
| Bill of materials | /AEB/WU\_PL\_CR\_01  |
| Purchase order    | /AEB/WU\_PO\_CR\_01  |
| Sales order       | /AEB/WU\_SO\_CR\_01  |
| Invoice           | /AEB/WU\_INV\_CR\_01 |

# Customer

BAdI method: /AEB/WU\_CUS\_CR\_01 - before\_standard\_filling (modify the parameters for how the data is populated by default)\
Parameters:

> IM\_KNA1 (customer master (general part))\
> IM\_ORG\_UNIT (organizational unit)\
> IM\_KNVKS (list of contact persons in customer master)\
> EX\_KNVK\_ADRND (business address)\
> EX\_SPRAS (language key)

BAdI method: /AEB/WU\_CUS\_CR\_01 - after\_standard\_filling (modify the data to be exported)\
Parameters:

> IM\_KNA1 (customer master (general part))\
> IM\_ORG\_UNIT (organizational unit)\
> IM\_KNVKS (list of contact persons in customer master)\
> CH\_ADDRESS\_DO (address)

# Vendor

BAdI method: /AEB/WU\_VDR\_CR\_01 - before\_standard\_filling (modify the parameters for how the data is populated by default)\
Parameters:

> IM\_LFA1 (supplier master (general part))\
> IM\_ORG\_UNIT (organizational unit)\
> IM\_LFM1S (list of purchasing organization data in supplier master)\
> EX\_EKORG (purchasing organization)

BAdI method: /AEB/WU\_VDR\_CR\_01 - after\_standard\_filling (modify the data to be exported)\
Parameters:

> IM\_LFA1 (supplier master (general part))\
> IM\_LFM1 (purchasing organization data in supplier master)\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_ADDRESS\_DO (address)

# Material

BAdI method: /AEB/WU\_MAT\_CR\_01 - before\_standard\_filling (modify the parameters for how the data is populated by default)\
Parameters:

> IM\_MARA (general material data)\
> IM\_MARCS (list of factory data for material)\
> IM\_MBEWS (list of material evaluation)\
> IM\_ORG\_UNIT (organizational unit)\
> EX\_MARC\_WERKS (factory)\
> EX\_MBEW\_BWKEY (valuation area)\
> EX\_MBEW\_BWTAR (valuation type)

BAdI method: /AEB/WU\_MAT\_CR\_01 - after\_standard\_filling (modify the data to be exported)\
Parameters:

> IM\_MARA (general material data)\
> IM\_MARC (factory data for material)\
> IM\_MBEW (material evaluation)\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_MATERIAL\_DO (material)

# Goods receipt

BAdI method: /AEB/WU\_GR\_CR\_01 - after\_standard\_filling (modify the data to be exported)\
Parameters:

> IM\_MKPF (material document header)\
> IM\_MSEGS (list of document segment for material)\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_GOODS\_RECEIPT\_DO (goods receipt)

# Incoming invoice

BAdI method: /AEB/WU\_II\_CR\_01 - after\_standard\_filling (modify the data to be exported)\
Parameters:

> IM\_RBKP (incoming invoice document header in customer master)\
> IM\_RSEGS (list of document items in incoming invoice)\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_GOODS\_RECEIPT\_DO (goods receipt)

# Bill of materials

BAdI method: /AEB/WU\_PL\_CR\_01 - before\_standard\_filling (modify the parameters for how the data is populated by default)\
Parameters:

> IM\_STKO (BOM header)\
> IM\_STPOS (list of bill of materials items)\
> IM\_ORG\_UNIT (organizational unit)\
> EX\_WERKS (factory)\
> EX\_MAST\_STLAN (use of bill of materials)\
> EX\_MBEW\_BWKEY (valuation area)\
> EX\_MBEW\_BWTAR (valuation type)

BAdI method: /AEB/WU\_PL\_CR\_01 - after\_standard\_filling (modify the data to be exported)\
Parameters:

> IM\_STKO (BOM header)\
> IM\_STPOS (list of bill of materials items)\
> IM\_ORG\_UNIT (organizational unit)\
> CH\_PARTS\_LIST\_DO (BOM header)

# Purchase order

BAdI method: /AEB/WU\_PO\_CR\_01 - after\_standard\_filling (modify the data to be exported)\
Parameters:

> IM\_EKKO (purchasing document header)\
> IM\_STPOS (list of purchasing document items)\
> IM\_ORG\_UNIT (organizational unit)\
> IM\_EXP\_SESSION\_ID (session ID)\
> CH\_PURCHASE\_ORDER\_DO (purchase order)

# Sales order

BAdI method: /AEB/WU\_SO\_CR\_01 - after\_standard\_filling (modify the data to be exported)\
Parameters:

> IM\_VBAK (Sales document: header data)\
> IM\_VBAPS (list of sales document: item data)\
> IM\_ORG\_UNIT (organizational unit)\
> IM\_EXP\_SESSION\_ID (session ID)\
> CH\_SALES\_ORDER\_DO (sales order)

# Invoice

BAdI methode: /AEB/WU\_INV\_CR\_01 - after\_standard\_filling (modify the data to be exported)\
Parameters:

> IM\_VBRK (Invoice: header data)\
> IM\_VBRPS (list of invoice: item data)\
> IM\_ORG\_UNIT (organizational unit)\
> IM\_EXP\_SESSION\_ID (session ID)\
> CH\_SALES\_ORDER\_DO (sales order)
