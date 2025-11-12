---
title: BAdIs to handle synchronized declarations
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
These BAdIs are called every time a declaration is received from the International Customs Integration Engine, which is everytime there is a change in declaration. 

| Document type                                  | BAdI                   |
| :--------------------------------------------- | :--------------------- |
| Delivery                                       | /AEB/AES\_ET\_SYNC\_04 |
| Shipment                                       | /AEB/AES\_ET\_SYNC\_05 |
| Invoice                                        | /AEB/AES\_ET\_SYNC\_06 |
| Purchase document                              | /AEB/AES\_ET\_SYNC\_07 |
| Incoming Invoice                               | /AEB/AES\_ET\_SYNC\_08 |
| Freight order                                  | /AE1/AES\_ET\_SYNC\_09 |
| Other collectors (manual created consignments) | /AEB/AES\_ET\_SYNC\_09 |
| Material document                              | /AEB/AES\_ET\_SYNC\_10 |

A typical usecase for this BAdI is to write the customs registration number in further database fields in your SAP System. In the following example you can see how it is written into the SAP shipment.

```text Update the customs registration number at SAP shipment
DATA:
    mrn_number        TYPE exti2,
    mrn_number_object TYPE REF TO /aeb/cl_01_char_35_nv.

  mrn_number_object = im_declaration->get_customs_registration_numbe( ).
  IF NOT mrn_number_object IS INITIAL.
    mrn_number = mrn_number_object->v.
    UPDATE vttk SET exti2 = mrn_number WHERE tknum = im_tknum.
  ENDIF.
```

If you have manual created consignments the link to the SAP document will be on item level. For that you have the possibility to access the client system ids on item level in the synchronization BADIs.\
In addition you have the public class /aeb/cl\_01\_pb\_tid\_def\_bc or /ae1/cl\_01\_pb\_tid\_def\_bc (S4 HANA specific objects like freight order). With this class you can convert the client system id to the SAP document number to access the SAP Document. 

```text Use item client system ids from Sync BADIs
METHOD /aeb/if_ex_aes_et_sync_09~hdl_declaration_synchronized.
    DATA: items       TYPE /aeb/if_aes_pb_decl_item_do=>tt_decl_item_do,
          curr_item   TYPE REF TO /aeb/if_aes_pb_decl_item_do,
          ids         TYPE /aeb/01_char255s,
          curr_id     TYPE /aeb/01_char255,
          curr_id_str TYPE string,
          vbeln       TYPE vbeln,
          likp        TYPE likp,
          vbrk        TYPE vbrk.

    items = im_declaration->get_items( ).
    LOOP AT items INTO curr_item.
      ids = curr_item->get_client_system_ids_v1( ).
      LOOP AT ids INTO curr_id.
        curr_id_str = curr_id.
        vbeln = /aeb/cl_01_pb_tid_def_bc=>new( )->get_sap_doc_no_from_tid( curr_id_str ).
        IF /aeb/cl_01_pb_tid_def_bc=>new( )->is_tid_from_likp( curr_id_str ) = 'X'.
          SELECT SINGLE * FROM likp WHERE vbeln = @vbeln INTO @likp.
        ELSEIF /aeb/cl_01_pb_tid_def_bc=>new( )->is_tid_from_vbrk( curr_id_str ) = 'X'.
          SELECT SINGLE * FROM vbrk WHERE vbeln = @vbeln INTO @vbrk.
        ENDIF.
      ENDLOOP.
    ENDLOOP.
  ENDMETHOD.
```

All of these BAdIs support the raising of the exception /aeb/cx\_aes\_pb\_et\_sync\_sc as it is described under [https://sap-plugins.docs.developers.aeb.com/docs/exceptions](https://sap-plugins.docs.developers.aeb.com/docs/exceptions). When the exception is raised, then the complete handling (also the standard handling) of the customs declaration is considered as failed, which means there will be an entry created in the function call monitor which can be automatically repeated with the report /AEB/01\_FCC\_RETRY if the customizing is done. When this repeat happens (either automatically by the report or manually by a user in the function call monitor) then the whole customs declaration is transferred from the engine and processed and also this BAdI will be called again.
