---
title: Order BOM
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
      slug: material-configured-in-sales-order-item
      title: Material configured in sales order item
---
[block:callout]
{
  "type": "info",
  "body": "Order BOMs are created in SAP transaction CS61."
}
[/block]
# Data export steps
1. Fill material
2. Fill BOM
3. Fill sales order

# Step details
## Fill material
BAdI /AEB/WU_PL_SO_CR_01 – after_standard_filling
## Fill BOM
BAdI /AEB/WU_PL_SO_CR_02 – after_standard_filling
## Fill sales order
BAdI /AEB/WU_SO_CR_01 - after_standard_filling