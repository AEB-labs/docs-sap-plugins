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
> 📘 Order BOMs are created in SAP transaction CS61.

# Data export steps

1. Fill material
2. Fill BOM
3. Fill sales order

# Step details

## Fill material

BAdI /AEB/WU\_PL\_SO\_CR\_01 – after\_standard\_filling

## Fill BOM

BAdI /AEB/WU\_PL\_SO\_CR\_02 – after\_standard\_filling

## Fill sales order

BAdI /AEB/WU\_SO\_CR\_01 - after\_standard\_filling
