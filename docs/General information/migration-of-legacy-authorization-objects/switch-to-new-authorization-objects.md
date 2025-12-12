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
3. Import the deletion report for the old authorization objects.

As long as an old authorization object is present in your system, it will be queried by the authorization check. Then new one will only be evaluated if the old authorization object does not exist anymore.
