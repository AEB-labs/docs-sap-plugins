---
title: Documents as attachments in the interface
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
Certain BAdIs allow the transfer of an attachment. This can be, for example, a PDF document, an image file, or another type of attachment.  For more details of the functionality, refer to [https://customsmanagement.docs.developers.aeb.com/docs/add-an-attachment](https://customsmanagement.docs.developers.aeb.com/docs/add-an-attachment)

Hint: the data type RAWSTRING corresponds to an uninterpreted byte string of variable length. The corresponding ABAP type is XSTRING.

```text Example in BAdI /AEB/AES_CONS_INV_01
DATA:
    attachment TYPE REF TO /aeb/if_aes_pb_attachment_do,
    file_stream TYPE /aeb/01_rawstring.
    
    
  CALL FUNCTION 'SCMS_BASE64_DECODE_STR'
      EXPORTING
        INPUT          = 'VEhJUyBJUyBKVVNUIEEgVEVTVCA='
     IMPORTING
       OUTPUT         = file_stream
     EXCEPTIONS
       FAILED         = 1
       OTHERS         = 2.

 attachment = im_cons_data_object_factory->new_aes_pb_attachment_do( im_attachment_code = 'Sample.txt'
                                                                      im_data = file_stream ).
im_value->add_attachment( attachment ).
```

Another helpful function in this context might be SCMS\_BINARY\_TO\_XSTRING.
