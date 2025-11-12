---
title: Instantiation of objects using the data object factory
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
New BAdIs no longer use structures as parameters, but objects with corresponding getter/setter methods for the individual fields or for other objects. To create instances of these objects, these BAdIs provide a so called "data object factory" as importing parameter, which has a corresponding instantiation method for each relevant object in the BAdI.
Example of an instantiation:

[block:code]
{
  "codes": [
    {
      "code": "new_item = im_data_object_factory->new_cmp_pb_ec_item_do( ).",
      "language": "text",
      "name": "Instantiation"
    }
  ]
}
[/block]