---
title: Material variant BOM
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
      slug: order-bom
      title: Order BOM
---
# Data export steps

1. Explode BOM using function module CS\_BOM\_EXPL\_MAT\_V2
2. Relevant for transfer?
3. Filling\
   3.1 Fill BOM on first level\
   3.2 For all items\
     3.2.1 Check if item is a configurable material, continue only if it is\
     3.2.2 Relevant for transfer?\
     3.2.3 Fill material\
     3.2.4 Fill BOM on current level

# Step details

## Relevant for transfer?

1. Check if head material is to export\
   1.1 Relevant type of rule: Definition of materials to be exported\
   1.2 Setting „Transfer bills of material as deleted in case of error"
2. Check if any item (on first level) is to export\
   2.1 Relevant type of rule: Definition of materials to be exported
3. BAdI /AEB/WU\_PLCMAT\_OU\_01 – is\_conf\_mat\_to\_export

## Fill BOM

BAdI /AEB/WU\_PLCMPL\_CR\_01 – after\_standard\_filling

## Fill material

BAdI /AEB/WU\_PLCMAT\_CR\_01 – after\_standard\_filling
