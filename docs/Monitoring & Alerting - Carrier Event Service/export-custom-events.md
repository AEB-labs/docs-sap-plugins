---
title: 'Sending event data '
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
## Transmission of customer specific events

You can send your own custom events from the SAP system to Monitoring & Alerting using the function module '/AEB/MA_PB_TF_TE_CUS'. &#x20;

<Accordion title="Parameters " icon="fa-info-circle">
  The function module has the following import parameters:

  IM_TRACKING_EVENT_DO (Event to be transmitted)<br />IM_ORG_UNIT (Organizational unit for Monitoring & Alerting)<br />IM_REF_NO_FOR_LOGGING (Reference number for logging)

  The function module has the following export parameters:

  EX_RESULT (Report of data transmission)
  EX_HAS_ERROR (Transmission errors occurred)
  EX_IS_TRANSFERRED (Object was transmitted)
</Accordion>

Make sure the shipment has been created in Monitoring & Alerting already, otherwise the event cannot be matched there.

The example below is sending an event with the event code "/AEB/SAP_GOODS_ISSUE".  It's using the shipment number in Monitoring & Alerting as a reference to the shipment object. Any other data of the event can be custom, just fill 'te_do' with relevant information you want to transfer.

```Text /AEB/MA_PB_TF_TE_CUS example coding
DATA:
  te_do           TYPE /aeb/ma_pb_te_20_do,
  reference       TYPE /aeb/ma_pb_te_ref_20_do,
  reference_field TYPE /aeb/ma_pb_ter20_ref_fd_do,
  result          TYPE /aeb/ma_pb_tf_msg_text,
  has_errors      TYPE /aeb/01_boolean,
  is_transferred  TYPE /aeb/01_boolean,
  timestamp       TYPE timestamp.

reference_field-reference_field = 'CONS_NO'.
reference_field-reference_number = '<SAP document numner, e.g. outbound delivery number>'.
APPEND reference_field TO reference-reference_fields.
reference-tracking_obj_type = 'CONS'.
APPEND reference TO te_do-references.

te_do-meta_data-id_ref_scheme = 'SUPPLIER'.
te_do-meta_data-message_reference_number = '<SAP document number, e.g. outbound delivery number>'.
te_do-meta_data-update_mode = 'STANDARD'.
te_do-meta_data-receiver_client = '<client of the target system>'.

te_do-head-identcode = '/AEB/SAP_GOODS_ISSUE'.
CONVERT DATE sy-datum TIME sy-timlo INTO TIME STAMP timestamp TIME ZONE 'CET'.
te_do-head-actualdate-date_in_timezone = timestamp.
te_do-head-actualdate-timezone = 'CET'.

CALL FUNCTION '/AEB/MA_PB_TF_TE_CUS'
  EXPORTING
    im_tracking_event_do  = te_do
    im_org_unit           = '< organizational unit as defined in the SAP add-on>'
    im_ref_no_for_logging = '<SAP document number>'
  IMPORTING
    ex_result             = result
    ex_has_error          = has_errors
    ex_is_transferred     = is_transferred
  EXCEPTIONS
    parameter_empty       = 1
    OTHERS                = 2.

IF sy-subrc <> 0.
* Implement suitable error handling here
ENDIF.
```

Example process:

The outbound delivery #80000111 is sent from SAP to  Monitoring & Alerting, creating a new shipment with number 80000111 there.  Afterwards, the event "PACKED" is send, using '80000111Ä as reference value for reference type "CONS_NO" (the shipment number).

<br />

## Transmission of the goods issue event&#x20;

If activated, the add-on automatically transmits an event about the goods issue for an outbound delivery to Monitoring & Alerting. Use the according BAdI /AEB/MA_DLV_EV_01 to prevent the transmission (method is_to_transfer ).

This BAdI runs whenever a relevant delivery is saved, regardless of whether the transmission of the business object to Monitoring & Alerting is triggered.

<Callout icon="📘" theme="info">
  ### Deprecated methods

  The following methods are deprecated and not approved for use: change_event and change_or&#x67;_\__&#x75;nit
</Callout>

<br />

<br />

<br />
