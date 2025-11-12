---
title: Handle MRN
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
> 🚧 Copy & Paste
>
> This is not a one solve all solution. Please make sure, that this fits to your processes.

```text Handle MRN
DATA:
  curr_as4data        TYPE t_cus_mrn_of_cons,
  curr_inv_no_struct  TYPE t_cus_mrn_inv,
  fld_comwa           TYPE vbco6,
  t_vbfas             TYPE TABLE OF vbfa,
  vbfa                TYPE vbfa,
  fld_pos_no          TYPE /aeb/cmp_pb_item_ref_id,
  curr_exnum          TYPE vbrk-exnum,
  curr_trans_no       TYPE vttk-tknum,
  fld_ref_no          TYPE posnr,
  t_ec_item           TYPE /aeb/cmp_pb_ec_req_cle_itms,
  ec_item             TYPE /aeb/cmp_pb_ec_req_cle_itm,
  t_ec_clearing       TYPE /aeb/cmp_pb_ec_clea_res_dos,
  curr_ec_clearing    TYPE /aeb/cmp_pb_ec_clea_res_do,
  c_lic_pos           TYPE REF TO /aeb/cl_cmp_pb_ec_lic_info_bc,
  t_lic_pos           TYPE STANDARD TABLE OF posnr_vl WITH DEFAULT KEY,
  curr_doc_no         TYPE /aeb/cmp_pb_doc_no,
  c_lics              TYPE REF TO /aeb/if_cmp_pb_ec_lici_res_do,
  t_lic_info_dos      TYPE /aeb/cmp_if_cmp_pb_ec_li_i_dos,
  curr_lic_info_do    TYPE REF TO /aeb/if_cmp_pb_ec_lic_info_do.

"Loop over different consignments / mrns
LOOP AT as4data-cus_mrn_of_cons INTO curr_as4data.
  "Loop over invoices of consignments
  LOOP AT curr_as4data-cus_mrn_inv INTO curr_inv_no_struct.
    "Determine delivery
    fld_comwa-mandt = sy-mandt.
    fld_comwa-vbeln = curr_inv_no_struct-inv_no.
    CALL FUNCTION 'RV_ORDER_FLOW_INFORMATION'
      EXPORTING
        comwa      = fld_comwa
        nachfolger = '-'
      TABLES
        vbfa_tab   = t_vbfas.
    READ TABLE t_vbfas INTO vbfa WITH KEY vbtyp_v = 'J'.

    "Determine shipment
    SELECT SINGLE tknum FROM vttp
      INTO curr_trans_no
      WHERE vbeln = vbfa-vbelv.

    "Write number in VA in CCO
    CALL FUNCTION '/AEB/PA_PB_DLV_COMPLETE_SHP'
      EXPORTING
        im_likp = curr_trans_no.

    "Try to get clearings of items
    SELECT posnr FROM lips
      INTO TABLE t_lic_pos
      WHERE vbeln = vbfa-vbelv.

    curr_doc_no = vbfa-vbelv.
    c_lic_pos = /aeb/cl_cmp_pb_ec_lic_info_bc=>new_for( curr_doc_no ).

    LOOP AT t_lic_pos INTO fld_pos_no.
      c_lics = c_lic_pos->get_license_info_for_item( fld_pos_no ).
      t_lic_info_dos = c_lics->get_lic_info_dos( ).
      LOOP AT t_lic_info_dos INTO curr_lic_info_do.
        IF curr_lic_info_do->get_license_id( ) IS NOT INITIAL.
          "Read EC clearing for customs office and date
          ec_item-doc_no = vbfa-vbelv.
          ec_item-item_ref_id = fld_pos_no.
          APPEND ec_item TO t_ec_item.
          CALL FUNCTION '/AEB/CMP_PB_EC_GET_CLEA_FOR'
            EXPORTING
              im_requests = t_ec_item
              im_org_unit = 'DE'
            IMPORTING
              ex_results  = t_ec_clearing.

          "Update EC clearing
          LOOP AT t_ec_clearing INTO curr_ec_clearing.
            CALL FUNCTION '/AEB/CMP_PB_EC_ADD_CI_FOR_DLV'
              EXPORTING
                im_mandt            = sy-mandt
                im_vbeln            = vbfa-vbelv
                im_posnr            = fld_ref_no
                im_export_date      = curr_ec_clearing-clearing_data-dateofexport
                im_cust_office      = curr_ec_clearing-clearing_data-customsoffice
                im_cust_accept_date = curr_ec_clearing-clearing_data-customsacceptancedate
                im_atlas_mrn        = curr_as4data-mrn.
          ENDLOOP.
          EXIT.
        ENDIF.
      ENDLOOP.
    ENDLOOP.

    "Update Fields in documents
    "Update invoice
    SELECT SINGLE exnum FROM vbrk
      INTO curr_exnum
      WHERE vbeln = curr_inv_no_struct-inv_no.

    UPDATE eikp
      SET text1 = curr_as4data-mrn
      WHERE exnum = curr_exnum.

    "Update shipment
    UPDATE vttk
      SET text3 = curr_as4data-mrn
      WHERE tknum = curr_trans_no.
  ENDLOOP.
ENDLOOP.
```
```text Full Example
TYPES: BEGIN OF t_cus_mrn_inv,
         inv_no TYPE string,
       END OF t_cus_mrn_inv.
TYPES: tt_cus_mrn_inv TYPE STANDARD TABLE OF t_cus_mrn_inv WITH DEFAULT KEY.
TYPES: BEGIN OF t_cus_mrn_of_cons,
         mrn         TYPE string,
         cus_mrn_inv TYPE tt_cus_mrn_inv,
       END OF t_cus_mrn_of_cons.
TYPES: tt_cus_mrn_of_cons TYPE STANDARD TABLE OF t_cus_mrn_of_cons WITH DEFAULT KEY.
TYPES: BEGIN OF t_document,
         cus_mrn_of_cons TYPE tt_cus_mrn_of_cons,
       END OF t_document.

DATA:
  result_messages_dto TYPE REF TO /aeb/if_ct_pb_nsg_doc_res_dto,
  bo_id               TYPE string,
  as4data             TYPE t_document,
  curr_as4data        TYPE t_cus_mrn_of_cons,
  curr_inv_no_struct  TYPE t_cus_mrn_inv,
  fld_comwa           TYPE vbco6,
  t_vbfas             TYPE TABLE OF vbfa,
  vbfa                TYPE vbfa,
  fld_pos_no          TYPE /aeb/cmp_pb_item_ref_id,
  curr_exnum          TYPE vbrk-exnum,
  curr_trans_no       TYPE vttk-tknum,
  fld_ref_no          TYPE posnr,
  t_ec_item           TYPE /aeb/cmp_pb_ec_req_cle_itms,
  ec_item             TYPE /aeb/cmp_pb_ec_req_cle_itm,
  t_ec_clearing       TYPE /aeb/cmp_pb_ec_clea_res_dos,
  curr_ec_clearing    TYPE /aeb/cmp_pb_ec_clea_res_do,
  c_lic_pos           TYPE REF TO /aeb/cl_cmp_pb_ec_lic_info_bc,
  t_lic_pos           TYPE STANDARD TABLE OF posnr_vl WITH DEFAULT KEY,
  curr_doc_no         TYPE /aeb/cmp_pb_doc_no,
  c_lics              TYPE REF TO /aeb/if_cmp_pb_ec_lici_res_do,
  t_lic_info_dos      TYPE /aeb/cmp_if_cmp_pb_ec_li_i_dos,
  curr_lic_info_do    TYPE REF TO /aeb/if_cmp_pb_ec_lic_info_do.

TRY.
    bo_id = im_journal_entry_group->get_bo_id( ).
    im_engine_if->get_cus_data( EXPORTING
                                  im_bf_name = 'As4Doc_BF_NAME'
                                  im_businessobjectid = bo_id
                                IMPORTING
                                  ex_result_dto = result_messages_dto
                                CHANGING
                                  ch_result_data_target = as4data ).
  CATCH /aeb/cx_01_pb_missing_parm_sc.
ENDTRY.

"Loop over different consignments / mrns
LOOP AT as4data-cus_mrn_of_cons INTO curr_as4data.
  "Loop over invoices of consignments
  LOOP AT curr_as4data-cus_mrn_inv INTO curr_inv_no_struct.
    "Determine delivery
    fld_comwa-mandt = sy-mandt.
    fld_comwa-vbeln = curr_inv_no_struct-inv_no.
    CALL FUNCTION 'RV_ORDER_FLOW_INFORMATION'
      EXPORTING
        comwa      = fld_comwa
        nachfolger = '-'
      TABLES
        vbfa_tab   = t_vbfas.
    READ TABLE t_vbfas INTO vbfa WITH KEY vbtyp_v = 'J'.

    "Determine shipment
    SELECT SINGLE tknum FROM vttp
      INTO curr_trans_no
      WHERE vbeln = vbfa-vbelv.

    "Write number in VA in CCO
    CALL FUNCTION '/AEB/PA_PB_DLV_COMPLETE_SHP'
      EXPORTING
        im_likp = curr_trans_no.

    "Try to get clearings of items
    SELECT posnr FROM lips
      INTO TABLE t_lic_pos
      WHERE vbeln = vbfa-vbelv.

    curr_doc_no = vbfa-vbelv.
    c_lic_pos = /aeb/cl_cmp_pb_ec_lic_info_bc=>new_for( curr_doc_no ).

    LOOP AT t_lic_pos INTO fld_pos_no.
      c_lics = c_lic_pos->get_license_info_for_item( fld_pos_no ).
      t_lic_info_dos = c_lics->get_lic_info_dos( ).
      LOOP AT t_lic_info_dos INTO curr_lic_info_do.
        IF curr_lic_info_do->get_license_id( ) IS NOT INITIAL.
          "Read EC clearing for customs office and date
          ec_item-doc_no = vbfa-vbelv.
          ec_item-item_ref_id = fld_pos_no.
          APPEND ec_item TO t_ec_item.
          CALL FUNCTION '/AEB/CMP_PB_EC_GET_CLEA_FOR'
            EXPORTING
              im_requests = t_ec_item
              im_org_unit = 'DE'
            IMPORTING
              ex_results  = t_ec_clearing.

          "Update EC clearing
          LOOP AT t_ec_clearing INTO curr_ec_clearing.
            CALL FUNCTION '/AEB/CMP_PB_EC_ADD_CI_FOR_DLV'
              EXPORTING
                im_mandt            = sy-mandt
                im_vbeln            = vbfa-vbelv
                im_posnr            = fld_ref_no
                im_export_date      = curr_ec_clearing-clearing_data-dateofexport
                im_cust_office      = curr_ec_clearing-clearing_data-customsoffice
                im_cust_accept_date = curr_ec_clearing-clearing_data-customsacceptancedate
                im_atlas_mrn        = curr_as4data-mrn.
          ENDLOOP.
          EXIT.
        ENDIF.
      ENDLOOP.
    ENDLOOP.

    "Update Fields in documents
    "Update invoice
    SELECT SINGLE exnum FROM vbrk
      INTO curr_exnum
      WHERE vbeln = curr_inv_no_struct-inv_no.

    UPDATE eikp
      SET text1 = curr_as4data-mrn
      WHERE exnum = curr_exnum.

    "Update shipment
    UPDATE vttk
      SET text3 = curr_as4data-mrn
      WHERE tknum = curr_trans_no.
  ENDLOOP.
ENDLOOP.
```
