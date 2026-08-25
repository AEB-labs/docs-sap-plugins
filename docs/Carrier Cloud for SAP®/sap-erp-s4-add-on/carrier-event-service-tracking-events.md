---
title: Carrier Event Service - tracking events
deprecated: false
hidden: false
metadata:
  robots: index
---
# Synchronization of tracking events

## Synchronization process&#x20;

Tracking events received from the carriers are synchronized back to your SAP system when executing the program /AEB/MA_CWT_SYNC. To run this program ferquently, schedule a job. &#x20;

The data is synchronized back to SAP in these tables:

| SAP table name | Description                                                                                               |
| :------------- | :-------------------------------------------------------------------------------------------------------- |
| /AEB/MA_E_CONS | Consignment (header data)                                                                                 |
| /AEB/MA_E_HU   | Handling unit (packages on highest level)                                                                 |
| /AEB/MA_E_TE   | Tracking events. Parent_ID is linked either to the ID of a consignment or to the ID  of an handling unit. |

## BAdI to handle synchronized tracking events

To handle the tracking data according to your requirements,  you can implement the BAdI /AEB/MA_EXP_CONS_01. The data structure provided in the BAdI is representing a consignment (shipment) with packages and events.

<Callout icon="📘" theme="info">
  ### Tracking events can be found at different levels

  The tracking events can be received for the whole shipment header and also for each handling unit. This depends on the carrier. So make sure you read the data from IM_CONSIGNMENT- EVENTS and also for IM_CONSIGNMENT-HANDLINGUNITS-EVENTS.
</Callout>

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

<br />

# Open list of tracking events&#x20;

To open the list of tracking events in Carrier Connect for a certain document, use one of the following classes.

| SAP document  | Class                    |
| :------------ | :----------------------- |
| Delivery      | /AEB/CL_PA_PB_DLV_DLS_AC |
| Shipment      | /AEB/CL_PA_PB_SHP_DLS_AC |
| Freight order | /AE1/CL_PA_PB_FRO_DLS_AC |

Code example:

```
DATA:
public_ac_class TYPE REF TO /AEB/CL_MA_PB_DLV_AF_AC. 

public_ac_class = /AEB/CL_MA_PB_DLV_AF_AC=>new_for( im_likp_vbeln =
```
