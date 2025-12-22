---
title: Process description
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
      slug: >-
        example-for-method-aebcl_wu_pb_pref_chk_inv_bc-determine_preference_texts
      title: Implementation examples
---
## Invoice printing

Examples: [https://sap-plugins.docs.developers.aeb.com/docs/example-for-method-aebcl_wu_pb_pref_chk_inv_bc-determine_preference_texts](https://sap-plugins.docs.developers.aeb.com/docs/example-for-method-aebcl_wu_pb_pref_chk_inv_bc-determine_preference_texts)

<Image border={false} src="https://files.readme.io/45fb475-invoice_printing.png" title="invoice_printing.png" />

# Explanation of single steps

## Start customer individual invoice printing procedure

The functions of the O&P invoice printing have to be integrated directly into the SAP invoice printing procedure.

## Determine preference texts

All document data are retrieved at runtime from the data base. After this, the data can be changed by a BAdI implementation. Now every item is checked whether the preference is achieved. Finally, the preference texts are generated. The requirements of the agreements with regard to language, name of the countries involved, and signature requirement are taken into account.

| Business object | Relevant types of rules              |
| :-------------- | :----------------------------------- |
| Invoice         | Determination of organizational unit |

Several functions are available:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Business object
      </th>

      <th>
        Function
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Invoice
      </td>

      <td>
        * Method /AEB/CL_WU_PB_PREF_CHK_INV_BC - determine_preference_texts (recommended) - Function module /AEB/WU_PB_DET_INV_PREF_TEXTS (deprecated)
      </td>
    </tr>

    <tr>
      <td>
        Delivery
      </td>

      <td>
        Method /AEB/CL_WU_PB_PREF_CHK_DLV_BC - determine_preference_texts
      </td>
    </tr>

    <tr>
      <td>
        Any other business object
      </td>

      <td>
        * Method /AEB/CL_WU_PB_PREF_CHK_CUS_BC - determine_preference_texts (recommended) - Function module /AEB/WU_PB_DET_CU_PREF_TEXTS (deprecated)
      </td>
    </tr>
  </tbody>
</Table>

| Business object           | BAdI method to change the data         |
| :------------------------ | :------------------------------------- |
| Invoice                   | /AEB/WU_DOC_INV_01 – after_std_filling |
| Sales order               | /AEB/WU_DOC_SD_01 - after_std_filling  |
| Delivery                  | /AEB/WU_DOC_DLV_01 - after_std_filling |
| Any other business object | /AEB/WU_DOC_CUS_01 – after_std_filling |

## Use preference texts on invoice

The determined preference texts have to be printed on the invoice. The head text can be placed at the beginning or the end of the invoice. Item texts should be printed at the corresponding item.

# Overview of functions

Besides the determination of preference texts, there are some additional methods in the classes /AEB/CL_WU_PB_PREF_CHK_INV_BC, /AEB/CL_WU_PB_PREF_CHK_DLV_BC and /AEB/CL_WU_PB_PREF_CHK_CUS_BC.

<Callout icon="📘" theme="info">
  The fields VBAK-KUNNR and VBRK-LAND1 are the basis for determining the partner/country.
</Callout>

### Method NEW_FOR

This method must be used to create instances.

### Method CHECK_IS_TO_SIGN

This method specifies if the invoice document with the declarations of origin must be signed manually.

### CHECK_ITEM_PREFERENCE

This method specifies for which item a declaration of origin may be submitted. The method returns the value of "X" if a declaration of origin may be submitted, and the value of '"-" if no declaration of origin may be  
submitted. The method can be called in the invoice printing process at runtime to trigger further processes  
relating to the declaration of origin according to the status of individual items.

### DETERMINE_PREFERENCE_TEXTS

**Requirements**  
The following conditions must be met before the method can select the correct preference text:

* The “ex-works price” is relevant for determining preference. The method needs to have this value indicated in the calculation subtotal field VBRP-KZWI5. This means that the “ex-works price” must already appear in this field when the preference text is selected. The ex-works price is to be specified for a quantity of 1.
* The invoice must already be posted in the SAP system – that is, available under the document number in the SAP database.
* The following logic is applied for returning the preference texts:
  * The preference text that occurs most frequently in the items is returned at the header level.
  * Only if a preference text at the item level differs from the text at the header level, it is also returned at the item level.
  * If no item is entitled to preference, no text is returned at the header level.

**Call**  
The method provides the preference texts for a document at runtime, so it can be integrated into the invoice printing process in order to print the correct preference texts.

### CHECK_IS_DECL_OF_ORI_ALLOWED

This method returns an indicator that specifies for the invoice if a declaration of origin may be submitted. The method returns the value of "X" if a declaration of origin may be submitted, and the value of '"-" if no declaration of origin may be submitted.

### DETERMINE_ITMS_ORIG_CTRYS

**Requirements**  
The invoice must already be posted in the SAP system – that is, available under the invoice number in the SAP database.  
**Call**  
The method provides the preferential origins of the invoice items if it is necessary to print these on the invoice item. The corresponding agreement provides if it is necessary.

### DETERMINE_ITEM_AWR_SRC_CTRY

This method determines the non-preferential country of origin ("non-preferential origin") from the Material Monitor threshold data (/AEB/WUMMDMON) for every item. If no value can be determined from the data, the country of origin from the material master is returned.

# Further functions for other documents

<br />

| Function module                | Description                                                   |
| :----------------------------- | :------------------------------------------------------------ |
| /AEB/WU_PB_CHK_CUS_DECL_OF_ORI | Determine if a declaration of origin is allowd for a document |
| /AEB/WU_PB_CHK_CUS_ITM_PREF    | Determines if an item is entitled to preference               |
| /AEB/WU_PB_CHK_IS_CUS_TO_SIGN  | Determines if a document has to be signed                     |
| /AEB/WU_PB_DET_CUS_AWR_CTRY    | Determines the non-pref. country of origin                    |
| /AEB/WU_PB_DET_CUS_ORIG_CTRYS  | Determines preferential origin for all items                  |
| /AEB/WU_PB_DET_CUS_PREF_TEXTS  | Determines the declaration of origin for a document           |
| /AEB/WU_PB_DET_SO_ITM_MAT_NO   | Determines the unique material number for a sales order item  |

| Class                | Description                                                                             |
| :------------------- | :-------------------------------------------------------------------------------------- |
| /AEB/CL_WU_PB_MMD_BC | Read the complete material (threshold value) entry from the business service management |
