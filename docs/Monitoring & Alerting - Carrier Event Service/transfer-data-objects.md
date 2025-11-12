---
title: Transfer data objects
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
The AEB Plug-in checks each delivery to determine whether it should be transmitted to Monitoring & Alerting. This decision can be modified using the following BAdIs.
[block:parameters]
{
  "data": {
    "h-0": "Data object",
    "h-1": "BAdI name",
    "h-2": "BAdI method",
    "0-0": "Sales order",
    "0-1": "/AEB/MA_SDOC_CR_01",
    "0-2": "IS_TO_TRANSFER",
    "1-0": "Delivery",
    "1-1": "/AEB/MA_DLV_CR_01",
    "1-2": "IS_TO_TRANSFER",
    "2-0": "Shipment",
    "2-1": "/AEB/MA_CWT_SHP_03",
    "2-2": "IS_TO_TRANSFER"
  },
  "cols": 3,
  "rows": 3
}
[/block]
Let's provide an example coding that shows how you can only transfer deliveries that are shipped to recievers inside the European Union.  
[block:code]
{
  "codes": [
    {
      "code": "METHOD /aeb/if_ex_ma_dlv_cr_01~is_to_transfer.\n    LOOP AT im_vbpas INTO DATA(ls_vbpa).\n      IF ls_vbpa-parvw = 'WE'.\n        SELECT SINGLE xegld FROM t005 WHERE land1 = @ls_vbpa-land1 INTO @DATA(lv_xegld).\n        IF lv_xegld IS INITIAL.\n          ch_is_to_transfer = '-'.\n        ENDIF.\n      ENDIF.\n    ENDLOOP.\n  ENDMETHOD.",
      "language": "text",
      "name": "Check ship-to eq EU "
    }
  ]
}
[/block]