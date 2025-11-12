---
title: Open questionnaire for SAP-Document
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
To open the questionnaire for a delivery or sales document, you can use the following methods from the public class /AEB/CL\_CMP\_PB\_OPEN\_QUEST\_AC.

## Open questionnaire for Delivery

```Text Open questionnaire for delivery
DATA:
	open_quest_ac TYPE REF TO /aeb/cl_cmp_pb_open_quest_ac.
  
  open_quest_ac = /aeb/cl_cmp_pb_open_quest_ac=>new( ).
  open_quest_ac->open_quest_for_dlv( im_vbeln = '0080000603' )
```

<br />

## Open questionnaire for Sales Document

```Text Open questionnaire for sales document
DATA:
	open_quest_ac TYPE REF TO /aeb/cl_cmp_pb_open_quest_ac.
  
  open_quest_ac = /aeb/cl_cmp_pb_open_quest_ac=>new( ).
  open_quest_ac->open_quest_for_sdoc( im_vbeln = '650' )
```
