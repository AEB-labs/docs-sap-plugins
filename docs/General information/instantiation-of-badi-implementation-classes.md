---
title: Instantiation of BAdI implementation classes
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
Since Build 05/2021 when AEB is calling BAdIs which may be implemented by you the BAdI implementation class is only instatiated once and then for every BAdI call the same instance is called. 
This means that you have to be carefull with using instance attributes within the BAdI implementation class as they can contain data from another document/master data etc. if you dont clear them yourself.