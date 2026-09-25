---
title: Transferring a document to M&A
excerpt: >-
  For the transfer of a freight order the class /AE1/CL_MA_PB_FRO_EXP_BC
  exists. 
deprecated: false
hidden: false
metadata:
  robots: index
---
```text
DATA:
  export_bc TYPE REF TO /ae1/cl_ma_pb_fro_exp_bc,
  result TYPE REF TO /AEB/IF_80_PB_EXP_RES_DO.

export_bc = /ae1/cl_ma_pb_fro_exp_bc=>new( ).
TRY.
  result = export_bc->do_export( im_tor_id = '618892671' ).
CATCH /AEB/CX_01_PB_MISSING_PARM_SC.
CATCH /AEB/CX_01_PB_EXPORT_FAILED_SC.
ENDTRY.

```
