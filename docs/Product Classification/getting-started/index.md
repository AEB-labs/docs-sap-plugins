---
title: Getting started
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: |-
    Like to understand more about the things happen when transfer material? 
    Or you like to know more about reading data from Classification Engine?
  pages:
    - type: basic
      slug: basic-conept-of-transfer-material
      title: Basic concept of transfer material
    - type: basic
      slug: basic-concept-of-read-material
      title: Basic concept of read material
---
This API documentation will help you to understand the basic concepts of the Product Classification plug-in for SAP and provides code snippets that explain how you can implement adaptions.

Let us start with the object model we're using in Product Classification. The main business object is the material. The following diagram shows the relation between the material and all other objects. 

![1212](https://files.readme.io/2019f7c-2019-12-02_161845.jpg "2019-12-02_161845.jpg")

Please keep two things in mind, when you working with the plug-in:\
**1. Organizational unit**\
Only materials which are assigned to an origanizational unit will be transferred to Product Classification.\
**2. Product group**\
Every material needs to be assigned to a product group. A missing product group will lead to errors. 

Both criterias are also used in the configuration to assign certain functionality and authorizations. 

The SAP plug-in for Product Classification provides two main functions:

* Transfer material records to Product Classification 
* Read material data from Product Classification and synchronize it back to the SAP master data
