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
If you like to exclude a material from transfer you have to assign no product group or you have to assign the product group with the value 'NOT_RELEVANT'.
For adapting the assignment of a product group, there is a BAdI /AEB/TA_PROD_GRP_01. The BAdI has one method chg_product_group_for_material.

The following code example shows how to set the product group in case of the material has a deletion indicator. 
[block:code]
{
  "codes": [
    {
      "code": "  METHOD /aeb/if_ex_ta_prod_grp_01~chg_product_group_for_material.\n\t\tIF im_mara-LVORM = 'X'\n  *   NOT_TO_TRANSFER has to be defined as product group\n\t\t\tch_product_group = 'NOT_RELEVANT'. \n    ENDIF.\n  ENDMETHOD.",
      "language": "text",
      "name": "Adapt determine product group"
    }
  ]
}
[/block]
You see that we only change it in this case, if the material has the deletion indicator. All other situations will be covered by configuration.

If you like to have context data for example of the material (MARA, MARC..) or if you like to have the relevant org. units use the parameter "IM_MATERIAL_CONTEXT". This parameter has some methods for example get_mara with which you can the actual content of the table mara (include the content which is not yet persistent).