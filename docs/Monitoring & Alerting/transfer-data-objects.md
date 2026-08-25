---
title: 'Define relevance for transmission  '
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
The AEB add-on checks for changes to an business object to decide whether it should be transmitted to Monitoring & Alerting. This decision can be modified using the following BAdIs.

| Data object | BAdI name          | BAdI method    |
| :---------- | :----------------- | :------------- |
| Sales order | /AEB/MA_SDOC_CR_01 | IS_TO_TRANSFER |
| Delivery    | /AEB/MA_DLV_CR_01  | IS_TO_TRANSFER |
| Shipment    | /AEB/MA_CWT_SHP_03 | IS_TO_TRANSFER |

Let's provide an example coding that shows how you can only transfer deliveries that are shipped to recievers inside the European Union.

```text Check ship-to eq EU 
METHOD /aeb/if_ex_ma_dlv_cr_01~is_to_transfer.
    LOOP AT im_vbpas INTO DATA(ls_vbpa).
      IF ls_vbpa-parvw = 'WE'.
        SELECT SINGLE xegld FROM t005 WHERE land1 = @ls_vbpa-land1 INTO @DATA(lv_xegld).
        IF lv_xegld IS INITIAL.
          ch_is_to_transfer = '-'.
        ENDIF.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.
```
