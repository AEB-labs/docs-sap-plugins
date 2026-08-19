---
title: BAdIs to change the exported data
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
If you would like to change the data of the shipment, use the following BAdIs to change the data.

| Document      | BAdI                                                                                                                                                                                               |
| :------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Delivery      | /AEB/MA_DLV_CR_01                                                                                                                                                                                  |
| Shipment      | /AEB/MA_CWT_SHP_01  - BEFORE_STANDARD_FILLING                                                                                                          /AEB/MA_CWT_SHP_02 - AFTER_STANDARD_FILLING |
| Freight order | /AE1/MA_CWT_FRO_01                                                                                                                                                                                 |
| Sales order   | /AEB/MA_SDOC_CR_01                                                                                                                                                                                 |

## Change shipment data

This coding example sets the email address of the consignee in the shipment:   &#x20;

```
LOOP AT im_vbpas INTO ls_vbpa.
  IF ls_vbpa-parvw = 'WE'.
    CALL FUNCTION 'ADDR_GET_COMPLETE'
      EXPORTING
        addrnumber              = ls_vbpa-adrnr
      IMPORTING
        addr1_complete          = ls_addr1_compl
      EXCEPTIONS
        parameter_error         = 1
        address_not_exist       = 2
        internal_error          = 3
        wrong_access_to_archive = 4
        address_blocked         = 5
        OTHERS                  = 6.
    IF sy-subrc <> 0.
      RETURN.
    ENDIF.

    LOOP AT ls_addr1_compl-adsmtp_tab INTO ls_adsmtp.
      IF ls_adsmtp-adsmtp-remark = 'AEB'.
        ch_cons_w_trans_do-head-consignee-email_address = ls_adsmtp-adsmtp-smtp_addr.
      ENDIF.
    ENDLOOP.

  ENDIF.
ENDLOOP.
```

<br />

## Change and create event data

To send event data from the SAP system , use the according BAdI

| Event             | BAdI               |
| :---------------- | :----------------- |
| Delivery event    | /AEB/MA_DLV_EV_01  |
| Sales order event | /AEB/MA_SDOC_EV_01 |

This BAdI runs whenever the delivery is saved, regardless of whether an update of the delivery is triggered.

<br />

The “change_event” method is not approved for use.&#x20;

<br />
