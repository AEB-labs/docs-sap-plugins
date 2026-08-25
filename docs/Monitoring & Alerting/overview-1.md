---
title: Overview
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This guide is applicable for Monitoring & Alerting completely. For the documentation about Carrier Event Service, please check the  <Anchor target="_blank" href="https://sap-plugins.docs.developers.aeb.com/docs/carrier-event-service-tracking-events">Carrier Event Service&#x20;</Anchor>documentation in the Carrier Cloud for SAP section.

# Data structure

The following diagram represents the data structure for the Monitoring & Alerting interface. For greater clarity, the diagram only shows the functionally relevant structures. The names of the segments are enclosed in parentheses. The square brackets indicate how many times a segment may be repeated. Bold segments are mandatory segments. Without them the structure is invalid.

Example: (Transport) \[0..n] means that there may be any number of transports specified per delivery (none is also allowed).

![](https://files.readme.io/eb016eb17596f38ec4f8b4d1b14927ddc736b597ae185b818c45dcc625b07c81-MA_data_structure.png)
