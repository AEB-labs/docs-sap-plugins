---
title: Switch to new authorization objects
deprecated: false
hidden: false
metadata:
  robots: index
---
For every legacy authorization object starting with 'ZAEB' there is a replacement authorization object starting with '/AEB/'. If you want to use the new objects, you have to perform these steps:

1. Add and maintain the new authorization objects to your authorization roles.
2. Remove the old authorization objects from the authorization roles.
3. Contact the AEB support and request the report ( program) to delete the old authorization objects. They will provide you a transport request for that. 

As long as a Z-authorization object is present in your system, it will be queried by the authorization check. The new object will only be evaluated if the according Z-authorization object does not exist anymore.
