---
title: Switch to new authorization objects (option C)
deprecated: false
hidden: false
metadata:
  robots: index
---
Option C: With this approach, you switch to new authorization objects and change the existing roles.  For every legacy authorization object starting with 'ZAEB' there is an according authorization object starting with '/AEB/' which replaces it. If you want to use the new /AEB/ objects, perform these steps:

1. Maintain the new authorization objects in your authorization roles. For a list of authorization objects, see the AEB document center: [Authorizations Add-on for SAP](https://docs.aeb.com/docs?type=contentMap\&custom_F897366923=897367691\&language=en\&custom_F894448523=894450827\&custom_format=excel)
2. Remove the old authorization objects from the authorization roles.
3. Contact the AEB support and request the report ( program) to delete the old authorization objects. They will provide you a transport request for that. The deletion is required because as long as the according ZAEB-authorization object is present in your system, it will be queried by the authorization check. The new /AEB/-object will only be evaluated if the according ZAEB-authorization object has been deleted.

<br />
