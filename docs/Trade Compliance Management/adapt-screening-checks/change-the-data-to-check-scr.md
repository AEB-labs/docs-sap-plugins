---
title: Change the data for screening checks
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
You can adapt the data to be checked by Compliance Screening using one of the following BAdI:

| BAdI name               | BAdI description                                                     |
| :---------------------- | :------------------------------------------------------------------- |
| /AEB/CMP\_SALES\_DC\_04 | Exclude certain partners in sales doument from check                 |
| /AEB/CMP\_SALES\_DC\_05 | Which partner no. (customer or vendor no) to use in sales documents  |
| /AEB/CMP\_DLV\_DC\_04   | Which partner no. (customer or vendor no) to use in delivery doc.    |
| /AEB/CMP\_SALES\_DC\_07 | Screening of a sales document: change data after standard filling    |
| /AEB/CMP\_DLV\_DC\_07   | Screening of a delivery: change data after standard filling          |
| /AEB/CMP\_PURCH\_DC\_03 | Screening of a purchase document: change data after standard filling |
| /AEB/CMP\_CUSTOMER\_05  | Screening of a customer: change data after standard filling          |
| /AEB/CMP\_DP\_DESCR\_01 | Determine the origin of an address                                   |
| /AEB/CMP\_DP\_DESCR\_02 | Determine the type of an address                                     |
| /AEB/CMP\_BUS\_PRT\_08  | Screening of a business partner: change data after standard filling  |

## Screening checks for address IDs

This example implementation uses the "after standard filling" method of an BAdI to add an address ID. Compliance checks can be executed for supported kinds of address IDs, e.g. a passport number, a tax number, DUNS number, etc.

```Text Add an address ID for the check
    DATA:
      address_ids TYPE /aeb/cmp_pb_address_id_dos,
      address_id  TYPE REF TO /aeb/if_cmp_pb_address_id_do.

    address_id = im_data_object_factory->new_cmp_pb_adress_id_do(
                   im_id_type  = 'PASSPORT_NO'
                   im_id_value = '7453987234' ).
    APPEND address_id TO address_ids.
    ch_doc_partners[ 1 ]-ids = address_ids.
```

For a list a valid ID types, look at [https://trade-compliance.docs.developers.aeb.com/reference/screenaddresses-1](https://trade-compliance.docs.developers.aeb.com/reference/screenaddresses-1) 

<Image align="center" src="https://files.readme.io/0cf08470a61b1fa486fef39886c001336dec8a079a0029c55103c34ab182a351-image.png" />

<br />

## Open the SAP object from the match handling of Trade Compliance Management

The comment (info) field in the match handling of Trade Compliance Management supports markup. When users are working with that application, you can make use of that and include a direct link to the SAP business object in this field: 

```Text Create a link to a sales order
    DATA:
      parameters TYPE tihttpnvp,
      parameter  TYPE ihttpnvp,
      url        TYPE string.
    FIELD-SYMBOLS:
     <curr_partner> TYPE /aeb/cmp_pb_doc_partner_do.

    parameter-name = 'SalesOrder'.
    parameter-value = im_vbak-vbeln.
    APPEND parameter TO parameters.

    url = cl_lsapi_manager=>create_flp_url( parameters = parameters
                                            object = 'SalesOrder'
                                            action = 'display' ).

    LOOP AT ch_doc_partners ASSIGNING <curr_partner>.
      <curr_partner>-info = url.
    ENDLOOP.
```
