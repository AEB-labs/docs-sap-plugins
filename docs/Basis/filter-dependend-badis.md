---
title: General functions
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
      slug: master-company-data-export
      title: Master Company Data Export
    - type: basic
      slug: restrict-background-task-usage-1
      title: Restrict background task usage
---
## Filter dependent BAdIs

In some cases, it's possible to use filter dependent BAdIs. They can be implemented multiple times. Each implementation then declares a filter value for which it should be called. At moment, the product is the only available filter criteria. In example,  to implement such a BAdI for the product Trade Compliance Management,  enter 'CMP' as the filter value.

## Shared Memory

It is possible to use the "Shared memory" functionality for the determination of application texts. Doing so, the table access to the tables DOKIL and DOKTL are reduced.\
To enable the shared memory, implement BAdI /AEB/01\_SHM\_01.
