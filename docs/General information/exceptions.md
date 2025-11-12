---
title: Exceptions
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
In newer BAdIs, it is also possible to generate an error message and thus interrupt processing, for example. Technically, this is done using object-oriented exceptions.\
-> The signature tells you whether an exception is available for a BAdI:

![](https://files.readme.io/befd649-Exception_Signature.png "Exception Signature.png")

The following example implementations refer to the BAdI /AEB/AES\_CONS\_INV\_01, but can also be transferred to other BAdIs that provide exceptions.

```text Interrupt processing without error message
IF im_vbrk-land1 = 'DE'.
    RAISE EXCEPTION TYPE /aeb/cx_aes_pb_cons_ete_sc.
  ENDIF.
```

Interrupt processing, with error message:

* Create a new subclass for the superclass of the exception. For example, a subclass with the name\
  ZCX\_AEB\_AES for the superclass /AEB/CX\_AES\_PB\_CONS\_ETE\_SC.
* Define the subclass as class type Exception class, deactivate the property "With messages of message classes as exception texts" and activate the Final property.
* Define the corresponding long texts for the required error messages in the created subclass in the\
  Texts folder. The newly created text IDs should begin with "ZZ", for example,\
  zzmessage\_wrong\_country.
* Save and activate the subclass.
* In the BAdI, call an exception for the new subclass and enter the text ID of the relevant long text for\
  the error message:

```text Interrupt processing with error message
IF im_vbrk-land1 = 'DE'.
    RAISE EXCEPTION TYPE zcx_aeb_aes
      EXPORTING 
        textid = zcx_aeb_aes=>zzmessage_wrong_country.
  ENDIF.
```

Interrupt processing, with error message including parameter value:

* Within the error message, it is possible to determine part of the displayed text dynamically using a\
  parameter.
* To do this, define a new variable in the subclass of the exception. (Folder Attributes, Type of attribute\
  is Instance Attribute, the visibility is public, Type corresponds to the intended value). Example:\
  Variable ZZVBELN, type VBELN.
* Use the variable when defining the long text, including % characters. Example for the content of a\
  long text: "Country for document %ZZVBELN% is not valid".
* In the BAdI, call the exception with the corresponding parameter:

```text Interrupt processing with error message including parameter value
IF im_vbrk-land1 = 'DE'.
    RAISE EXCEPTION TYPE zcx_aeb_aes
      EXPORTING 
        textid = zcx_aeb_aes=>zzmessage_wrong_country
        zzvbeln = im_vbrk-vbeln.
  ENDIF.
```
