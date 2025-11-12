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

| Document type                 | BAdI                                                                                                                                                                                                                |
| :---------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Delivery                      | /AEB/MA\_DLV\_CR\_01                                                                                                                                                                                                |
| Events created for a delivery | /AEB/MA\_DLV\_EV\_01                                                                                                                                                                                                |
| Shipment                      | /AEB/MA\_CWT\_SHP\_01  - BEFORE\_STANDARD\_FILLING  <br />                                                                                                         /AEB/MA\_CWT\_SHP\_02 - AFTER\_STANDARD\_FILLING |
| Freight order                 | /AE1/MA\_CWT\_FRO\_01                                                                                                                                                                                               |
| Sales order                   | /AEB/MA\_SDOC\_CR\_01                                                                                                                                                                                               |

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
