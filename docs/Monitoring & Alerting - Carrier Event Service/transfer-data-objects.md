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

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Data object
      </th>

      <th style={{ textAlign: "left" }}>
        BAdI name
      </th>

      <th style={{ textAlign: "left" }}>
        BAdI method
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Sales order
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/MA\_SDOC\_CR\_01
      </td>

      <td style={{ textAlign: "left" }}>
        IS\_TO\_TRANSFER
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Delivery
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/MA\_DLV\_CR\_01
      </td>

      <td style={{ textAlign: "left" }}>
        IS\_TO\_TRANSFER
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Shipment
      </td>

      <td style={{ textAlign: "left" }}>
        /AEB/MA\_CWT\_SHP\_03
      </td>

      <td style={{ textAlign: "left" }}>
        IS\_TO\_TRANSFER
      </td>
    </tr>
  </tbody>
</Table>

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
