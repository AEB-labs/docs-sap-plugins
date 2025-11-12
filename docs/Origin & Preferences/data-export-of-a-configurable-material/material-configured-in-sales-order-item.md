---
title: Material configured in sales order item
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
      slug: badi-definitions-1
      title: BAdI definitions
---
# Data export steps
1. Explode BOM using function module CS_BOM_EXPL_KND_V1
2. Relevant for transfer?
3. Fill material
4. Fill BOM on first level
4.1 For all items
4.2 Check if item is a configurable material, continue only if it is
4.3 Relevant for transfer?
4.4 Fill material
4.5 Fill BOM on current level

# Step details
## Relevant for transfer?
1. Check if head material is to export
1.1 Relevant type of rule: Definition of materials to be exported
2. Check if any item (on first level) is to export
2.1 Relevant type of rule: Definition of materials to be exported
3. BAdI /AEB/WU_SOCMAT_OU_01 – is_so_conf_mat_to_export

## Fill material
BAdI /AEB/WU_SOCMAT_CR_01 – after_standard_filling
## Fill BOM
BAdI /AEB/WU_SOCMPL_CR_01 – after_standard_filling