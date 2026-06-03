---
title: Change the assigned product group
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
  pages:
    - type: basic
      slug: trigger-the-transfer-of-material
      title: Trigger the transfer of Material
    - type: basic
      slug: adapt-material-data-to-transfer
      title: Adapt material data to transfer
---
The BAdI /AEB/TA_PROD_GRP_01 runs after the standard identification of the product group. This BAdI allows to define a deviating product group.

Note: The new product group must have been configured first, otherwise materials with this group will not be transmitted to Product Classification.

If you want to exclude a material from the transfer, assign the product group with the value 'NOT_RELEVANT'.

The following code example shows how to set the product group in case of the material has a deletion indicator.

```text Adapt determine product group
METHOD /aeb/if_ex_ta_prod_grp_01~chg_product_group_for_material.
		IF im_mara-LVORM = 'X'
  *   NOT_RELEVANT has to be defined as product group
			ch_product_group = 'NOT_RELEVANT'. 
    ENDIF.
  ENDMETHOD.
```

To access material-related data, for example from tables such as MARA and MARC, or to retrieve relevant organizational unit data, use the parameter `IM_MATERIAL_CONTEXT`. This parameter provides several methods, such as `GET_MARA`, which retrieves the current content of the MARA structure, including data that has not yet been persisted to the database.

<br />

<br />

***

<br />

***

<br />
