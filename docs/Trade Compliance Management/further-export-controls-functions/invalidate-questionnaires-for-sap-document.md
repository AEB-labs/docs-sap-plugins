---
title: Invalidate questionnaires for SAP Document
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
To invalidate the questionnaires for a SAP document, you can use the method INVALIDATE\_QUESTIONNAIRES from the public class /AEB/CL\_CMP\_PB\_QUEST\_BC. The template ID is optional and can be left empty, when all questionnaires of one SAP document should be invalidated. 

```Text Invalidate Questionnaires
DATA:
	quest bc TYPE REF TO /aeb/cl_cmp_pb_quest_bc.

  quest_bc = /aeb/cl_cmp_pb_quest_bc=>new_for( document_number ).
  
  quest_bc->invalidate_questionnaires( questionnaire_template_id ).
```
