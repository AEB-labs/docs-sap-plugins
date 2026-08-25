---
title: Synchronization of tracking events
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
## Synchronization job

Tracking events received from your carriers are synchronized back to SAP when executing the program /AEB/MA_CWT_SYNC.

The data is synchronized back to SAP in these tables:

| SAP table name | Description                                                                                               |
| :------------- | :-------------------------------------------------------------------------------------------------------- |
| /AEB/MA_E_CONS | Consignment (header data)                                                                                 |
| /AEB/MA_E_HU   | Handling unit (packages on highest level)                                                                 |
| /AEB/MA_E_TE   | Tracking events. Parent_ID is linked either to the ID of a consignment or to the ID  of an handling unit. |

<br />

## BAdI to handle tracking events

To handle the tracking data according to your requirements,  you can implement the BAdI /AEB/MA_EXP_CONS_01. The data structure provided in the BAdI is representing a shipment (consignment) from Monitoring & Alerting with packages and events.

<Callout icon="📘" theme="info">
  ### Tracking events can be found at different levels

  The tracking events can be received for the whole shipment header and also for each handling unit. This depends on the carrier. So make sure you read the data from IM_CONSIGNMENT- EVENTS and also for IM_CONSIGNMENT-HANDLINGUNITS-EVENTS.
</Callout>

Example coding of handling events in the BAdI:

```Text Handle synchronized events
 _im_consignment = im_consignment.
LOOP AT _im_consignment-events INTO _event.
      "Ignore planned events
      IF _event-actualdate IS INITIAL.
        CONTINUE.
      ENDIF.
     * Your logic to handle the event data.
    ENDLOOP.
```

##
