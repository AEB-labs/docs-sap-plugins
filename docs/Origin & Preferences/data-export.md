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

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/45a68c1-use_case_wup_diagram.jpg",
        "use_case_wup_diagram.jpg",
        338
      ],
      "align": "center",
      "sizing": "auto"
    }
  ]
}
[/block]


# Explanation of single steps

## Selection of business objects

Depending on the values in the filter fields of the selection screen all data objects are selected, that match the criteria. Customizing settings are not considered at this time.

## Handling of a single business object

The following steps are performed for every selected business object.

## Check for changes

If the delta synchronization is activated on the selection screen, a check is performed, whether a change happened since the last transfer. The corresponding BAdI implementations are also checked.

| Business object  | BAdI method                                                          |
| :--------------- | :------------------------------------------------------------------- |
| Customer         | /AEB/WU_CUS_CC_01 -> has_changed                                     |
| Vendor           | /AEB/WU_VDR_CC_01 -> has_changed                                     |
| Material         | /AEB/WU_MAT_CC_01 -> has_changed                                     |
| Bill of material | - /AEB/WU_PL_CC_01 -> has_changed - /AEB/WU_MAT_CC_01 -> has_changed |
| Purchase order   | /AEB/WU_PO_CC_01 -> has_changed                                      |
| Goods receipt    | /AEB/WU_GR_CC_01 -> has_changed                                      |
| Incoming invoice | /AEB/WU_II_CC_01 -> has_changed                                      |
| Sales order      | /AEB/WU_SO_CC_01 -> has_changed                                      |
| Invoice          | /AEB/WU_INV_CC_01 -> has_changed                                     |

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

| Business object  | BAdI method                                      |
| :--------------- | :----------------------------------------------- |
| Customer         | /AEB/WU_CUS_OU_01 – is_customer_to_export        |
| Vendor           | /AEB/WU_VDR_OU_01 – is_vendor_to_export          |
| Material         | /AEB/WU_MAT_OU_01 – is_material_to_export        |
| Bill of material | /AEB/WU_PL_OU_01 – is_parts_list_to_export       |
| Purchase order   | /AEB/WU_PO_OU_01 – is_purchase_order_to_export   |
| Goods receipt    | /AEB/WU_GR_OU_01 – is_goods_receipt_to_export    |
| Incoming invoice | /AEB/WU_II_OU_01 – is_incoming_invoice_to_export |
| Sales order      | /AEB/WU_SO_OU_01 – is_sales_order_to_export      |
| Invoice          | /AEB/WU_INV_OU_01 - is_invoice_to_export         |

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

| Business object  | BAdI method                                                                                |
| :--------------- | :----------------------------------------------------------------------------------------- |
| Customer         | - /AEB/WU_CUS_CR_01 – before_standard_filling - /AEB/WU_CUS_CR_01 – after_standard_filling |
| Vendor           | - /AEB/WU_VDR_CR_01 – before_standard_filling - /AEB/WU_VDR_CR_01 – after_standard_filling |
| Material         | - /AEB/WU_MAT_CR_01 – before_standard_filling - /AEB/WU_MAT_CR_01 – after_standard_filling |
| Bill of material | - /AEB/WU_PL_CR_01 – before_standard_filling - /AEB/WU_PL_CR_01 – after_standard_filling   |
| Purchase order   | /AEB/WU_PO_CR_01 – after_standard_filling                                                  |
| Goods receipt    | /AEB/WU_GR_CR_01 - after_standard_filling                                                  |
| Incoming invoice | /AEB/WU_II_CR_01 - after_standard_filling                                                  |
| Sales order      | /AEB/WU_SO_CR_01 - after_standard_filling                                                  |
| Invoice          | /AEB/WU_INV_CR_01 - after_standard_filling                                                 |

### Amount fields

If you want to change an amount field, you'll need to differentiate between 

- transfering a certain value for the amount 
- or transfering an value of 0.000 for the amount  
- or leave the amount field completely empty 

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
> You can use function module RS_VARIANT_CONTENTS to get the data entered in a variant. The name of the variant used in an export run can be retrieved from the system variable SY-SLSET