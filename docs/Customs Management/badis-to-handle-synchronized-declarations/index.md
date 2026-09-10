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
These BAdIs are called every time a declaration is received from the International Customs Integration service. A declaration is synchronized every time it has been changed.

| Document type                                      | BAdI                |
| :------------------------------------------------- | :------------------ |
| Delivery                                           | /AEB/AES_ET_SYNC_04 |
| Shipment                                           | /AEB/AES_ET_SYNC_05 |
| Invoice                                            | /AEB/AES_ET_SYNC_06 |
| Purchase document                                  | /AEB/AES_ET_SYNC_07 |
| Incoming Invoice                                   | /AEB/AES_ET_SYNC_08 |
| Freight order                                      | /AE1/AES_ET_SYNC_09 |
| Other collectors (for manual created consignments) | /AEB/AES_ET_SYNC_09 |
| Material document                                  | /AEB/AES_ET_SYNC_10 |

A typical use case for this BAdI is to populate additional database fields in your SAP system with the customs registration number. The example below illustrates how to store this value in the "External ID 2"-field of a shipment (German: Transport).

```text Write MRN into a field of a shipment
  DATA:
    mrn_number        TYPE exti2,
    mrn_number_object TYPE REF TO /aeb/cl_01_char_35_nv.

*Read the MRN object from synchronized data. The object is of type /AEB/CL_01_CHAR_35_NV
  mrn_number_object = im_declaration->get_customs_registration_numbe( ).

 "Assign value of the MRN number to a variable
  IF NOT mrn_number_object IS INITIAL.
    mrn_number = mrn_number_object->v.

  "Define your own logic here according to your requirements
    UPDATE vttk SET exti2 = mrn_number WHERE tknum = im_tknum.

  ENDIF.
```

### ID numbers of the SAP documents

If you have created consignments in ICI manually the link to the SAP document will be on item level. Therefore, the client system IDs can be accessed on item level in the synchronization BADIs.<br />In addition you have the public class /aeb/cl_01_pb_tid_def_bc or /ae1/cl_01_pb_tid_def_bc ( for SAP S/4HANA specific objects like freight order). With this class you can convert the client system id to the SAP document number to access the SAP document.

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

<br />

### Exceptions&#x20;

All listed BAdIs support raising the exception `/AEB/CX_AES_PB_ET_SYNC_SC` as documented at <Anchor target="_blank" href="https://sap-plugins.docs.developers.aeb.com/docs/exceptions](https://sap-plugins.docs.developers.aeb.com/docs/exceptions">Handle exceptions.</Anchor> When this exception is raised, the entire handling of the customs declaration—including standard processing—is treated as failed, resulting in an entry in the Function Call Monitor (FCC).  Provided the relevant configuration is in place, this entry can be reprocessed automatically using the report `/AEB/01_FCC_RETRY`.  Upon reprocessing (whether triggered automatically by the report or manually via the Function Call Monitor), the complete customs declaration is retransferred from the engine and processed again, and the BAdI is invoked once more.

### List of fields

The folllowing fields are returned in the synchronization per declaration:&#x20;

| Technical field name       | Description                                            |
| -------------------------- | ------------------------------------------------------ |
| DECL_PROCEDURE_TYPE_CODE   | Declaration procedure type                             |
| CLIENT_SPECIFIC_FIELDS     | Customer-specific fields                               |
| ADDITIONAL_REFERENCE1      | Additional reference                                   |
| AMOUNTS_OF_MONEY           | Amounts of money                                       |
| ATTACHMENTS                | Documents                                              |
| BUSINESS_OBJECT_ID         | Business object ID                                     |
| CLIENT_SYSTEM_ID           | Installation ID                                        |
| COMMERCIAL_REFERENCE_NUMBE | Commercial reference number                            |
| CONSIGNMENT_ID_CLIENT_SYST | Host system ID                                         |
| CONSIGNMENT_NUMBER         | Consignment number                                     |
| CUSTOMS_OFFICES            | Customs offices                                        |
| CUSTOMS_PROCESS_CODE       | E-customs system                                       |
| CUSTOMS_REGISTRATION_NUMBE | Customs registration number                            |
| DATES                      | Dates                                                  |
| DECISIVE_DATE              | Decisive date                                          |
| DECLARATION_TYPECODE       | Type of customs declaration                            |
| DECLARATION_VERSION        | Customs declaration version                            |
| DELIVERY_NUMBER            | Delivery number                                        |
| DELIVERY_REFERENCES        | Reference numbers of the deliveries in the declaration |
| DESTINATION_COUNTRY_CODE   | Destination country code                               |
| DISPATCH_COUNTRY_CODE      | Country of dispatch code                               |
| INVOICE_NUMBER             | Invoice number                                         |
| IS_DELETED                 | Deleted flag                                           |
| IS_VALID                   | Validity flag                                          |
| ITEMS                      | List of items                                          |
| LOCAL_REFERENCE            | Local reference number (LRN)                           |
| ORG_UNIT                   | Organizational unit                                    |
| PARTIES                    | List of parties                                        |
| PERSON_IN_CHARGE           | Contact                                                |
| STATUS                     | Status                                                 |
| TOTAL_GROSSMASS            | Gross weight                                           |
| TOTAL_INVOICE_PRICE        | Invoice total amnount                                  |
| TOTAL_NETMASS              | Net weight                                             |
| TRANSPORT_MEANS            | Modes of transport                                     |
| INTERNAL_REFERENCE_V1      | Internal reference                                     |

The folllowing fields are returned in the synchronization per declaration item:

| Technical field name       | Description                    |
| -------------------------- | ------------------------------ |
| CUSTOMS_SPECIAL_PROCEDURES | Customs special procedures     |
| CUSTOMS_PROCEDURES         | Customs procedures             |
| CLIENT_SPECIFIC_FIELDS     | List of client specific fields |
| AMOUNTS_OF_MONEY           | Amounts of money               |
| CLASSIFICATIONS            | List of classifications        |
| GOODS_DESCRIPTION          | Goods description              |
| GROSSMASS                  | Gross weight                   |
| NETMASS                    | Net weight                     |
| PARTIES                    | List of parties                |
| QUANTITIES                 | Quantities                     |
| SEQUENCE_NUMBER            | Declaration item number        |
| STATUS                     | Status                         |
| CLIENT_SYSTEM_IDS_V1       | Host system IDs                |
