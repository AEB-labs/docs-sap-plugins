---
title: Export custom events
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
You can send your own custom events from SAP to Monitoring & Alerting using the function module '/AEB/MA_PB_TF_TE_CUS'.  Make sure the shipment has been created in Monitoring & Alerting already, otherwise the event cannot be matched there.

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

Outbound delivery #80000111 is sent from SAP to  Monitoring & Alerting, creating a new shipment with number  80000111 there.

Afterwards, you're sending the event "PACKED" using the value 80000111 as reference value for reference type "CONS_NO"  (shipment number).