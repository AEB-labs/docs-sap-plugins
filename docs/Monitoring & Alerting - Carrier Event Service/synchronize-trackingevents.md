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

Tracking events received from your carriers are synchronized back to SAP when executing the program /AEB/MA\_CWT\_SYNC.  

The data is synchronized back to SAP in these tables: 

| SAP table name   | Description                                                                                                |
| :--------------- | :--------------------------------------------------------------------------------------------------------- |
| /AEB/MA\_E\_CONS | Consignment (header data)                                                                                  |
| /AEB/MA\_E\_HU   | Handling unit (packages on highest level)                                                                  |
| /AEB/MA\_E\_TE   | Tracking events. Parent\_ID is linked either to the ID of a consignment or to the ID  of an handling unit. |

<br />

## BAdI to handle tracking events

To handle the tracking data according to your requirements,  you can implement the BAdI /AEB/MA\_EXP\_CONS\_01. The data structure provided in the BAdI is representing a consignment (shipment) with packages and events.

> 📘 Tracking events can be found at different levels
>
> The tracking events can be received for the whole shipment header and also for each handling unit. This depends on the carrier. So make sure you read the data from IM\_CONSIGNMENT- EVENTS and also for IM\_CONSIGNMENT-HANDLINGUNITS-EVENTS.

<br />

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

## Mapping of fields (Carrier Event Service)

If the tracking data is provided from Carrier Event Service, you can see the mapping of fields in the consignment of  Business Service Management (BSM).  Start the application "Consignments" in BSM, open of the consignments and use F1 key to open the help information:     

![](https://files.readme.io/87a7ffc9272fa793498dbfcf9c8c11aa31db721aee840bf205cffc8279a599f5-image.png)
