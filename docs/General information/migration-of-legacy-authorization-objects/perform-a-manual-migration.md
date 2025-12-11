---
title: Perform a manual migration
deprecated: false
hidden: true
metadata:
  robots: index
---
To move the objects, first create the new package. Do not select any of the ZAEB_*_NAMESPACELESS_OBJECTS packages. You could use ZAEB_DEPRECATED_OBJECTS or any other package name starting with 'Z'.

After that, you can use the Repository Information System to move the authorization objects into the package. You need development rights for namespace 'Z*'. Open the Object Navigator (SE80) and select the Repository Information System. Open "Other objects -> Authorization Objects". Use the select option of the field "Authorization Object" to enter the following list of objects. Use the function "Upload from clipboard" to conveniently insert the objects. Execute the selection. On the result list, select all entries and perform the menu function "Auth. objects -> Reassign". Enter the new package name and execute the function.

| Authorization object list |
| :------------------------ |
| adsf                      |
| sdf                       |
| agd                       |
| asdf                      |

<br />
