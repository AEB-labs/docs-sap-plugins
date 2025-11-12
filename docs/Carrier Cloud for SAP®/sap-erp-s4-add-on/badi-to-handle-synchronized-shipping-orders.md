---
title: BAdI to handle synchronized shipping orders
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
When a shipping order  gets updated in Carrier Cloud, the data can be synchronized back to SAP by a job that executes the program /AEB/PA_DLS_SYNC.  To access this data and implement your own logic, you can use the BAdI /AEB/PA_DLS_01.

Here is a short example to write the recieved carrier shipment number into the BOLNR of the delivery:

```text Update LIKP-BOLNR
DATA:
  vbelns TYPE TABLE OF vbeln,
  vbeln  TYPE vbeln,
  tknum  TYPE tknum.
FIELD-SYMBOLS:
  <curr_vbeln> TYPE vbeln.

IF im_shipment-carriershipmentnumber IS NOT INITIAL.
  CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
    EXPORTING
      input  = im_shipment-referencenumber1
    IMPORTING
      output = tknum.
  SELECT vbeln
    FROM vttp
    INTO TABLE @vbelns
    WHERE tknum = @tknum.
  IF vbelns IS INITIAL.
    vbeln = tknum.
    APPEND vbeln TO vbelns.
  ENDIF.
  LOOP AT vbelns ASSIGNING <curr_vbeln>.
    UPDATE likp
    SET bolnr = im_shipment-carriershipmentnumber
    WHERE vbeln = <curr_vbeln>.
  ENDLOOP.
ENDIF.
```

Here is an example how to save the tracking number in the freight order (if the shipping order was created from a freight order).

```text
DATA:
      tor_guid        TYPE /bobf/conf_key,
      tor_id          TYPE /scmtms/tor_id,
      service_manager TYPE REF TO /bobf/if_tra_service_manager,
      message         TYPE REF TO /bobf/if_frw_message,
      modifications   TYPE /bobf/t_frw_modification,
      modification    TYPE /bobf/s_frw_modification,
      tordrf          TYPE /scmtms/d_tordrf,
      exc             TYPE REF TO /bobf/cx_frw_contrct_violation,
      longtext        TYPE string,
      atf_key_mapping TYPE /scmtms/cl_common_helper=>ty_atf_key_mapping,
      tmp_tordrf      TYPE /scmtms/d_tordrf.

    IF im_shipment-carriershipmentnumber IS NOT  INITIAL.
      CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
        EXPORTING
          input  = im_shipment-referencenumber1
        IMPORTING
          output = tor_id.

      SELECT SINGLE db_key FROM /scmtms/d_torrot INTO tor_guid WHERE tor_id = tor_id.
      IF sy-subrc <> 0.
        RETURN.
      ENDIF.

      SELECT SINGLE * FROM /scmtms/d_tordrf INTO tmp_tordrf WHERE parent_key = tor_guid
                                                              AND btd_tco = 'T67'.
      IF sy-subrc = 0.
        RETURN.
      ENDIF.
      tordrf-parent_key = tor_guid.
      tordrf-btd_tco = 'T67'.
      tordrf-btd_id = im_shipment-carriershipmentnumber. "carrier tracking number.
      tordrf-btd_date = sy-datum.
      tordrf-btd_issuer = 'AEB Carrier Connect'.

      GET REFERENCE OF tordrf INTO modification-data.
      APPEND /scmtms/if_tor_c=>sc_node_attribute-docreference-node_data TO modification-changed_fields.

      modification-key = tor_guid.
      modification-change_mode = /bobf/if_frw_c=>sc_modify_create.
      modification-node        = /scmtms/if_tor_c=>sc_node-docreference.
      modification-node_cat = /scmtms/if_tor_c=>sc_node_category-docreference-docreference.
      modification-association = /scmtms/if_tor_c=>sc_association-root-docreference.
      modification-source_key = tor_guid.
      modification-source_node = /scmtms/if_tor_c=>sc_node-root.

      APPEND modification TO modifications.

      TRY .
          service_manager = /bobf/cl_tra_serv_mgr_factory=>get_service_manager( /scmtms/if_tor_c=>sc_bo_key ).
          service_manager->modify( EXPORTING it_modification = modifications
                                  IMPORTING eo_message      = message ).
          /bobf/cl_tra_trans_mgr_factory=>get_transaction_manager( )->save( IMPORTING eo_message = message ).
        CATCH /bobf/cx_frw_contrct_violation INTO exc.
          longtext = exc->if_message~get_longtext( ).
      ENDTRY.
    ENDIF.
```