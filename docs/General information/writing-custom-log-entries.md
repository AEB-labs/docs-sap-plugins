---
title: Writing Custom Log Entries
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
Traceability and transparency is an important factor for customer developments. For this reason we provide public classes to create log entries into the AEB log application, so they can be seen with one look together with the standard logs by AEB. This also includes all the standard features of the AEB log applications like mail distribution and the authorization concept. 

| Product                     | Class                        |
| :-------------------------- | :--------------------------- |
| Customs Management          | /AEB/CL\_AES\_PB\_LOGGER\_BC |
| Trade Compliance Management | /AEB/CL\_CMP\_PB\_LOGGER\_BC |
| AEB Connector               | /AEB/CL\_CT\_PB\_LOGGER\_BC  |
| Carrier Connect             | /AEB/CL\_PA\_PB\_LOGGER\_BC  |
| Product Classification      | /AEB/CL\_TA\_PB\_LOGGER\_BC  |
| Origin & Preferences        | /AEB/CL\_WU\_PB\_LOGGER\_BC  |
| Monitoring & Alerting       | /AEB/CL\_MA\_PB\_LOGGER\_BC  |

```text Example call to write a log
DATA:
      logger       TYPE REF TO /aeb/cl_cmp_pb_logger_bc,
      logtext      TYPE string.

    logtext = 'A fatal error occured: Exception 500 - technical error in connection.'.

    logger = /aeb/cl_cmp_pb_logger_bc=>new( ).
    logger->create_log_critical( im_org_unit = '1000'
                                 im_ref_no   = '123'
                                 im_log_text = logtext ).
```
