---
title: Restrict background task usage
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
## Scenario

When you want to use background tasks to check multiple business objects parallel you might overload the connected engine, because the SAP system is sending to many requests per second. To limit how many requests are sent you can either resize the logon group /AEB/DEFAULT or implement the BAdI /AEB/01\_QTSK\_CTRL\_01. Resizing the logon group is easier but also not as exact, because you can only specify a percentage of processes which could be used. The BAdI on the other hand allows you to specify the exact amount of processes to be used and even allows you to change it depending on the use case.

## BAdI /AEB/01\_QTSK\_CTRL\_01

The parameter of the BAdI is a  runtime context object. It offers the following methods:

* GET\_RUNTIME\_CONTEXT (Provides the runtime context from which the task was created)
* GET\_USED\_SERVER\_GROUP\_NAME (Provides the used server group)
* GET\_SERVER\_GROUP\_MAX\_WPS\_COUNT (Provides the maximum number of processes of the server group)
* GET\_MAX\_WPS\_TO\_USE\_COUNT (Provides the maximum number of processes to be used)
* GET\_INITIAL\_FREE\_WPS\_COUNT (Provides the number of free processes at initialization)
* SET\_MAX\_WPS\_TO\_USE\_COUNT (Sets the maximum number of processes to be used)

You can simply set the maximum number of processes to the amount that should be used.

```text Limit bank check
METHOD /aeb/if_ex_01_qtsk_ctrl_01~change_spbt_runtime_parms.
  IF im_spbt_runtime_parms->get_runtime_context( ) = /aeb/cl_cmp_pb_spbt_contexts=>c_cmp_bank_check. "If the bank check is run
    im_spbt_runtime_parms->set_max_wps_to_use_count( 2 ). "Limit processes to 2
  ENDIF.
ENDMETHOD.
```
