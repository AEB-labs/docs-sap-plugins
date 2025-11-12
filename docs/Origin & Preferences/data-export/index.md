---
title: Data export
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: badis-to-determine-whether-an-object-has-changed
      title: BAdIs to determine whether an object has changed
    - type: basic
      slug: badis-to-mark-objects-for-data-export
      title: BAdIs to mark objects for data export
    - type: basic
      slug: badis-to-change-object-data
      title: BAdIs to change object data
---
The overall process is shown in the following flow diagram. It is common for all business objects.

<Image title="use_case_wup_diagram.jpg" alt={338} align="center" width="auto" src="https://files.readme.io/45a68c1-use_case_wup_diagram.jpg" />

# Explanation of single steps

## Selection of business objects

Depending on the values in the filter fields of the selection screen all data objects are selected, that match the criteria. Customizing settings are not considered at this time.

## Handling of a single business object

The following steps are performed for every selected business object.

## Check for changes

If the delta synchronization is activated on the selection screen, a check is performed, whether a change happened since the last transfer. The corresponding BAdI implementations are also checked.

| Business object  | BAdI method                                                                  |
| :--------------- | :--------------------------------------------------------------------------- |
| Customer         | /AEB/WU\_CUS\_CC\_01 -> has\_changed                                         |
| Vendor           | /AEB/WU\_VDR\_CC\_01 -> has\_changed                                         |
| Material         | /AEB/WU\_MAT\_CC\_01 -> has\_changed                                         |
| Bill of material | - /AEB/WU\_PL\_CC\_01 -> has\_changed - /AEB/WU\_MAT\_CC\_01 -> has\_changed |
| Purchase order   | /AEB/WU\_PO\_CC\_01 -> has\_changed                                          |
| Goods receipt    | /AEB/WU\_GR\_CC\_01 -> has\_changed                                          |
| Incoming invoice | /AEB/WU\_II\_CC\_01 -> has\_changed                                          |
| Sales order      | /AEB/WU\_SO\_CC\_01 -> has\_changed                                          |
| Invoice          | /AEB/WU\_INV\_CC\_01 -> has\_changed                                         |

## Relevant for transfer

Different checks are performed to see if the business object is relevant. Customizing rules and BAdI implementations are considered here.

| Business object  | Relevant types of rules                                                                                                                                                                                                      |
| :--------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Customer         | Determination of organizational unit                                                                                                                                                                                         |
| Vendor           | Determination of organizational unit                                                                                                                                                                                         |
| Material         | - Determination of organizational unit - Definition of materials to be exported                                                                                                                                              |
| Bill of material | - Determination of organizational unit - Definition of materials to be exported                                                                                                                                              |
| Purchase order   | - Determination of organizational unit - Definition of materials to be exported                                                                                                                                              |
| Goods receipt    | - Determination of organizational unit - Definition of materials to be exported - Definition of goods receipt items to be exported - Setting if goods receipt items without reference to orders are to be transmitted or not |
| Incoming invoice | - Determination of organizational unit - Definition of materials to be exported                                                                                                                                              |
| Sales order      | - Determination of organizational unit - Definition of materials to be exported                                                                                                                                              |
| Invoice          | - Determination of organizational unit - Definition of materials to be exported                                                                                                                                              |

| Business object  | BAdI method                                             |
| :--------------- | :------------------------------------------------------ |
| Customer         | /AEB/WU\_CUS\_OU\_01 – is\_customer\_to\_export         |
| Vendor           | /AEB/WU\_VDR\_OU\_01 – is\_vendor\_to\_export           |
| Material         | /AEB/WU\_MAT\_OU\_01 – is\_material\_to\_export         |
| Bill of material | /AEB/WU\_PL\_OU\_01 – is\_parts\_list\_to\_export       |
| Purchase order   | /AEB/WU\_PO\_OU\_01 – is\_purchase\_order\_to\_export   |
| Goods receipt    | /AEB/WU\_GR\_OU\_01 – is\_goods\_receipt\_to\_export    |
| Incoming invoice | /AEB/WU\_II\_OU\_01 – is\_incoming\_invoice\_to\_export |
| Sales order      | /AEB/WU\_SO\_OU\_01 – is\_sales\_order\_to\_export      |
| Invoice          | /AEB/WU\_INV\_OU\_01 - is\_invoice\_to\_export          |

## Filling

In the filling the data records for transmission to Origin & Preferences are created. For that purpose the data contents are determined from the SAP standard and the AEB data structures are filled with them. Before the filling by the AEB logic you can define the SAP data to use by BAdI implementation (for most business objects). After filling it is possible to change the created data by BAdI. Finally the data records are transferred.

| Business object  | Relevant types of rules                                                                                                                                         |
| :--------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Customer         | None                                                                                                                                                            |
| Vendor           | None                                                                                                                                                            |
| Material         | - Determination of organizational unit - Determination of commodity code for material - Determination of the procurement type - Determination of purchase price |
| Bill of material | Setting „Transfer bills of material as deleted in case of error“                                                                                                |
| Purchase order   | None                                                                                                                                                            |
| Goods receipt    | None                                                                                                                                                            |
| Incoming invoice | None                                                                                                                                                            |
| Sales order      | None                                                                                                                                                            |
| Invoice          | None                                                                                                                                                            |

| Business object  | BAdI method                                                                                          |
| :--------------- | :--------------------------------------------------------------------------------------------------- |
| Customer         | - /AEB/WU\_CUS\_CR\_01 – before\_standard\_filling - /AEB/WU\_CUS\_CR\_01 – after\_standard\_filling |
| Vendor           | - /AEB/WU\_VDR\_CR\_01 – before\_standard\_filling - /AEB/WU\_VDR\_CR\_01 – after\_standard\_filling |
| Material         | - /AEB/WU\_MAT\_CR\_01 – before\_standard\_filling - /AEB/WU\_MAT\_CR\_01 – after\_standard\_filling |
| Bill of material | - /AEB/WU\_PL\_CR\_01 – before\_standard\_filling - /AEB/WU\_PL\_CR\_01 – after\_standard\_filling   |
| Purchase order   | /AEB/WU\_PO\_CR\_01 – after\_standard\_filling                                                       |
| Goods receipt    | /AEB/WU\_GR\_CR\_01 - after\_standard\_filling                                                       |
| Incoming invoice | /AEB/WU\_II\_CR\_01 - after\_standard\_filling                                                       |
| Sales order      | /AEB/WU\_SO\_CR\_01 - after\_standard\_filling                                                       |
| Invoice          | /AEB/WU\_INV\_CR\_01 - after\_standard\_filling                                                      |

### Amount fields

If you want to change an amount field, you'll need to differentiate between 

* transfering a certain value for the amount 
* or transfering an value of 0.000 for the amount  
* or leave the amount field completely empty 

```text Transfer 100.00 EUR as value in context of BAdI-Method /AEB/IF_EX_WU_GR_CR_01~AFTER_STANDARD_FILLING
FIELD-SYMBOLS: <item> type /AEB/WU_PB_GRI_DO.

    loop at ch_goods_receipt_do-items ASSIGNING <item>.
      <item>-currency = 'EUR'.
      <item>-value = '100.000'.
    endloop.
```

```text Transfer 0.00 EUR as value in context of BAdI-Method /AEB/IF_EX_WU_GR_CR_01~AFTER_STANDARD_FILLING
FIELD-SYMBOLS: <item> type /AEB/WU_PB_GRI_DO.

    loop at ch_goods_receipt_do-items ASSIGNING <item>.
      <item>-currency = 'EUR'.
      <item>-value = '0.000'.
    endloop.
```

```text Transfer no value in context of BAdI-Method /AEB/IF_EX_WU_GR_CR_01~AFTER_STANDARD_FILLING
FIELD-SYMBOLS: <item> type /AEB/WU_PB_GRI_DO.

    loop at ch_goods_receipt_do-items ASSIGNING <item>.
      <item>-currency = ''. "just leave the currency empty... 
      <item>-value = '0.000'.
    endloop.
```

> 📘 Read data for the used variant
>
> You can use function module RS\_VARIANT\_CONTENTS to get the data entered in a variant. The name of the variant used in an export run can be retrieved from the system variable SY-SLSET
