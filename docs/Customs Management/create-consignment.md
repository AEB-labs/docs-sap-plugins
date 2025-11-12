---
title: Trigger the data transfer
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
To transfer business objects from the SAP system to Customs Management, one can either use output messages (NAST framework) or the public APIs of the add-on.  
This section lists the public API functions that can be used in a context of a transaction document to trigger the data transfer. Choose the according object class for instantiation based on the document type:

| Transaction document type | Class to trigger the data transfer |
| :------------------------ | :--------------------------------- |
| Billing document          | /AEB/CL_AES_PB_INV_EXP_BC          |
| Purchase document         | /AEB/CL_AES_PB_PD_EXP_BC           |
| Delivery document         | /AEB/CL_AES_PB_DLV_EXP_BC          |
| Shipment document         | /AEB/CL_AES_PB_SHP_EXP_BC          |
| Incoming invoice          | /AEB/CL_AES_PB_II_EXP_BC           |
| Freight order             | /AE1/CL_AES_PB_FRO_EXP_BC          |
| Material document         | /AEB/CL_AES_PB_MD_EXP_BC           |

You can use the function _do_export_ to trigger the data transfer. See the following examples for each kind of transaction document (scroll to the right to see all):

```text Example coding to start a consignment for an invoice document
DATA:
  export_bc TYPE REF TO /aeb/cl_aes_pb_inv_exp_bc,
  vbeln     TYPE vbeln,
  cx_parm   TYPE REF TO /aeb/cx_01_pb_missing_parm_sc,
  cx_exp		TYPE REF TO /aeb/cx_01_pb_export_failed_sc.

  vbeln = '12345'.

export_bc = /aeb/cl_aes_pb_inv_exp_bc=>new( ).
TRY.
  export_bc->do_export( im_vbeln = vbeln ).
CATCH /aeb/cx_01_pb_missing_parm_sc INTO cx_parm.
  * a parameter is missing
CATCH /aeb/cx_01_pb_export_failed_sc INTO cx_exp.
	* export failed
ENDTRY.
```
```text Example coding for a purchase document
DATA:
  export_bc TYPE REF TO /aeb/cl_aes_pb_pd_exp_bc,
  ebeln     TYPE ebeln,
  cx_parm   TYPE REF TO /aeb/cx_01_pb_missing_parm_sc,
  cx_exp		TYPE REF TO /aeb/cx_01_pb_export_failed_sc.

  ebeln = '12345'.
  export_bc = /aeb/cl_aes_pb_pd_exp_bc=>new( ).
TRY.
	export_bc->do_export( im_ebeln = ebeln ).
CATCH /aeb/cx_01_pb_missing_parm_sc INTO cx_parm.
  * a parameter is missing
CATCH /aeb/cx_01_pb_export_failed_sc INTO cx_exp.
	* export failed
ENDTRY.
```
```text Example coding for a delivery document
DATA:
  export_bc TYPE REF TO /aeb/cl_aes_pb_dlv_exp_bc,
  vbeln     TYPE vbeln,
  cx_parm   TYPE REF TO /aeb/cx_01_pb_missing_parm_sc,
  cx_exp		TYPE REF TO /aeb/cx_01_pb_export_failed_sc.

	vbeln = '12345'.
	export_bc = /aeb/cl_aes_pb_dlv_exp_bc=>new( ).
TRY.
	export_bc->do_export( im_vbeln = vbeln ).
CATCH /aeb/cx_01_pb_missing_parm_sc INTO cx_parm.
	* a parameter is missing
CATCH /aeb/cx_01_pb_export_failed_sc INTO cx_exp.
	* export failed
ENDTRY.
```
```text Example coding for a shipment document
DATA:
  export_bc TYPE REF TO /aeb/cl_aes_pb_shp_exp_bc,
  tknum     TYPE tknum,
  cx_parm   TYPE REF TO cx_01_pb_missing_parm_sc,
  cx_exp		TYPE REF TO cx_01_pb_export_failed_sc.

  tknum = '12345'.
  export_bc = /aeb/cl_aes_pb_shp_exp_bc=>new( ).
TRY.
	export_bc->do_export( im_vttk_tknum = tknum ).
CATCH cx_01_pb_missing_parm_sc INTO cx_parm.
	* a parameter is missing
CATCH cx_01_pb_export_failed_sc INTO cx_exp.
	* export failed
ENDTRY.
```
```text Example coding for an incoming invoice
DATA:
  export_bc TYPE REF TO /aeb/cl_aes_pb_ii_exp_bc,
  belnr     TYPE re_belnr,
  gjahr     TYPE gjahr,
  cx_parm   TYPE REF TO cx_01_pb_missing_parm_sc,
  cx_exp		TYPE REF TO cx_01_pb_export_failed_sc.


  belnr = '12345'.
  gjahr = '2022'.

	export_bc = /aeb/cl_aes_pb_ii_exp_bc=>new( ).
TRY.
	export_bc->do_export( im_belnr = belnr
												im_gjahr = gjahr ).
CATCH cx_01_pb_missing_parm_sc INTO cx_parm.
	* a parameter is missing
CATCH cx_01_pb_export_failed_sc INTO cx_exp.
	* export failed
ENDTRY.
```
```text Example coding for a material document
DATA:
  export_bc TYPE REF TO /aeb/cl_aes_pb_md_exp_bc,
  mblnr     TYPE mblnr,
  mjahr     TYPE mjahr,
  cx_parm   TYPE REF TO cx_01_pb_missing_parm_sc,
  cx_exp		TYPE REF TO cx_01_pb_export_failed_sc.

  mblnr = '12345'.
  mjahr = '2022'.

  export_bc = /aeb/cl_aes_pb_md_exp_bc=>new( ).
TRY.
  export_bc->do_export( im_mblnr = mblnr
  											im_mjahr = mjahr ).
CATCH cx_01_pb_missing_parm_sc INTO cx_parm.
	* a parameter is missing
CATCH cx_01_pb_export_failed_sc INTO cx_exp.
	* export failed
ENDTRY.
```
```text Example coding for a freight order
DATA:
  export_bc            TYPE REF TO /ae1/cl_aes_pb_fro_exp_bc,
  tor_id               TYPE /scmtms/tor_id,
  cx_parm   TYPE REF TO cx_01_pb_missing_parm_sc,
  cx_exp		TYPE REF TO cx_01_pb_export_failed_sc.

	tor_id = '12345'.

	export_bc = /ae1/cl_aes_pb_fro_exp_bc=>new( ).
TRY.
	export_bc->do_export( im_tor_id = tor_id ).
CATCH cx_01_pb_missing_parm_sc INTO cx_parm.
  * a parameter is missing
CATCH cx_01_pb_export_failed_sc INTO cx_exp.
	* export failed
ENDTRY.
```