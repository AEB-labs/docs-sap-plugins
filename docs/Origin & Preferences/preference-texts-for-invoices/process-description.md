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

Examples: [https://sap-plugins.docs.developers.aeb.com/docs/example-for-method-aebcl\_wu\_pb\_pref\_chk\_inv\_bc-determine\_preference\_texts](https://sap-plugins.docs.developers.aeb.com/docs/example-for-method-aebcl_wu_pb_pref_chk_inv_bc-determine_preference_texts)

![](https://files.readme.io/45fb475-invoice_printing.png "invoice_printing.png")

# Explanation of single steps

## Start customer individual invoice printing procedure

The functions of the O\&P invoice printing have to be integrated directly into the SAP invoice printing procedure.

## Determine preference texts

All document data are retrieved at runtime from the data base. After this, the data can be changed by a BAdI implementation. Now every item is checked whether the preference is achieved. Finally, the preference texts are generated. The requirements of the agreements with regard to language, name of the countries involved, and signature requirement are taken into account.

| Business object | Relevant types of rules              |
| :-------------- | :----------------------------------- |
| Invoice         | Determination of organizational unit |

Several functions are available:

| Business object           | Function                                                                                                                                                    |
| :------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Invoice                   | - Method /AEB/CL\_WU\_PB\_PREF\_CHK\_INV\_BC - determine\_preference\_texts (recommended) - Function module /AEB/WU\_PB\_DET\_INV\_PREF\_TEXTS (deprecated) |
| Any other business object | Function module /AEB/WU\_PB\_DET\_CUS\_PREF\_TEXTS                                                                                                          |

| Business object           | BAdI method to change the data              |
| :------------------------ | :------------------------------------------ |
| Invoice                   | /AEB/WU\_DOC\_INV\_01 – after\_std\_filling |
| Sales order               | /AEB/WU\_DOC\_SD\_01 - after\_std\_filling  |
| Delivery                  | /AEB/WU\_DOC\_DLV\_01 - after\_std\_filling |
| Any other business object | /AEB/WU\_DOC\_CUS\_01 – after\_std\_filling |

## Use preference texts on invoice

The determined preference texts have to be printed on the invoice. The head text can be placed at the beginning or the end of the invoice. Item texts should be printed at the corresponding item.

# Overview of functions for invoices

Besides the determination of preference texts, there are some additional methods in class /AEB/CL\_WU\_PB\_PREF\_CHK\_INV\_BC.

> 📘
>
> The fields VBAK-KUNNR and VBRK-LAND1 are the basis for determining the partner/country.

### Method NEW\_FOR

This method must be used to create instances.

### Method CHECK\_IS\_TO\_SIGN

This method specifies if the invoice document with the declarations of origin must be signed manually.

### CHECK\_ITEM\_PREFERENCE

This method specifies for which item a declaration of origin may be submitted. The method returns the value of "X" if a declaration of origin may be submitted, and the value of '"-" if no declaration of origin may be\
submitted. The method can be called in the invoice printing process at runtime to trigger further processes\
relating to the declaration of origin according to the status of individual items.

### DETERMINE\_PREFERENCE\_TEXTS

**Requirements**\
The following conditions must be met before the method can select the correct preference text:

* The “ex-works price” is relevant for determining preference. The method needs to have this value indicated in the calculation subtotal field VBRP-KZWI5. This means that the “ex-works price” must already appear in this field when the preference text is selected. The ex-works price is to be specified for a quantity of 1.
* The invoice must already be posted in the SAP system – that is, available under the document number in the SAP database.
* The following logic is applied for returning the preference texts:
  * The preference text that occurs most frequently in the items is returned at the header level.
  * Only if a preference text at the item level differs from the text at the header level, it is also returned at the item level.
  * If no item is entitled to preference, no text is returned at the header level.

**Call**\
The method provides the preference texts for a document at runtime, so it can be integrated into the invoice printing process in order to print the correct preference texts.

### CHECK\_IS\_DECL\_OF\_ORI\_ALLOWED

This method returns an indicator that specifies for the invoice if a declaration of origin may be submitted. The method returns the value of "X" if a declaration of origin may be submitted, and the value of '"-" if no declaration of origin may be submitted.

### DETERMINE\_ITMS\_ORIG\_CTRYS

**Requirements**\
The invoice must already be posted in the SAP system – that is, available under the invoice number in the SAP database.\
**Call**\
The method provides the preferential origins of the invoice items if it is necessary to print these on the invoice item. The corresponding agreement provides if it is necessary.

### DETERMINE\_ITEM\_AWR\_SRC\_CTRY

This method determines the non-preferential country of origin ("non-preferential origin") from the Material Monitor threshold data (/AEB/WUMMDMON) for every item. If no value can be determined from the data, the country of origin from the material master is returned.

# Further functions for other documents

<br />

| Function module                      | Description                                                   |
| :----------------------------------- | :------------------------------------------------------------ |
| /AEB/WU\_PB\_CHK\_CUS\_DECL\_OF\_ORI | Determine if a declaration of origin is allowd for a document |
| /AEB/WU\_PB\_CHK\_CUS\_ITM\_PREF     | Determines if an item is entitled to preference               |
| /AEB/WU\_PB\_CHK\_IS\_CUS\_TO\_SIGN  | Determines if a document has to be signed                     |
| /AEB/WU\_PB\_DET\_CUS\_AWR\_CTRY     | Determines the non-pref. country of origin                    |
| /AEB/WU\_PB\_DET\_CUS\_ORIG\_CTRYS   | Determines preferential origin for all items                  |
| /AEB/WU\_PB\_DET\_CUS\_PREF\_TEXTS   | Determines the declaration of origin for a document           |
| /AEB/WU\_PB\_DET\_SO\_ITM\_MAT\_NO   | Determines the unique material number for a sales order item  |

| Class                    | Description                                                                             |
| :----------------------- | :-------------------------------------------------------------------------------------- |
| /AEB/CL\_WU\_PB\_MMD\_BC | Read the complete material (threshold value) entry from the business service management |
