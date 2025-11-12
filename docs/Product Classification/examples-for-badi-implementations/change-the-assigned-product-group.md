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
If you like to exclude a material from transfer you have to assign no product group or you have to assign the product group with the value 'NOT\_RELEVANT'.\
For adapting the assignment of a product group, there is a BAdI /AEB/TA\_PROD\_GRP\_01. The BAdI has one method chg\_product\_group\_for\_material.

The following code example shows how to set the product group in case of the material has a deletion indicator. 

```text Adapt determine product group
METHOD /aeb/if_ex_ta_prod_grp_01~chg_product_group_for_material.
		IF im_mara-LVORM = 'X'
  *   NOT_TO_TRANSFER has to be defined as product group
			ch_product_group = 'NOT_RELEVANT'. 
    ENDIF.
  ENDMETHOD.
```

You see that we only change it in this case, if the material has the deletion indicator. All other situations will be covered by configuration.

If you like to have context data for example of the material (MARA, MARC..) or if you like to have the relevant org. units use the parameter "IM\_MATERIAL\_CONTEXT". This parameter has some methods for example get\_mara with which you can the actual content of the table mara (include the content which is not yet persistent).
