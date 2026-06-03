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
# Scenario

<br />

When using background tasks to execute functions in parallel, there is a risk to overload the Compliance engine, because the SAP system is sending too many requests in a certain time. To limit the number of parallel requests there are two options:

* Adjust the resources available for logon groups for the affected application servers. AEB does not recommend this approach, as it is a complex setup and has side effects of all related application servers. Keep in mind that future changes to the SAP system, e.g. increasing the available processes will also increase the number of parallel calls.
* Implement the BAdI /AEB/01_QTSK_CTRL_01 which allows you to specify the exact amount of processes to be used for Compliance checks and even allows you to change it depending on the use case. This is the recommend option, and works specifically for this requirement.

# BAdI /AEB/01_QTSK_CTRL_01

The parameter of the BAdI is a runtime context object. It offers the following methods:

* GET_RUNTIME_CONTEXT (Provides the runtime context from which the task was created)
* GET_USED_SERVER_GROUP_NAME (Provides the used server group)
* GET_SERVER_GROUP_MAX_WPS_COUNT (Provides the maximum number of processes of the server group)
* GET_MAX_WPS_TO_USE_COUNT (Provides the maximum number of processes to be used)
* GET_INITIAL_FREE_WPS_COUNT (Provides the number of free processes at initialization)
* SET_MAX_WPS_TO_USE_COUNT (Sets the maximum number of processes to be used)

You can set the maximum number of processes to the amount that should be used. Here is an example for the job execution of /AEB/01_FCC_RETRY_JOB.

```Text Limit processes for /AEB/01_FCC_RETRY_JOB
METHOD /aeb/if_ex_01_qtsk_ctrl_01~change_spbt_runtime_parms.
  IF im_spbt_runtime_parms->get_runtime_context( ) = /aeb/cl_01_pb_spbt_contexts=>c_01_fcc_retry_job. "If the job is running
    im_spbt_runtime_parms->set_max_wps_to_use_count( 2 ). "Limit processes to 2
  ENDIF.
ENDMETHOD.
```

Other supported contexts can be found in the corresponding context classes:

* /AEB/CL_01_PB_SPBT_CONTEXTS for functions available to all add-ons
* /AEB/CL_CMP_PB_SPBT_CONTEXTS for functions of Trade Compliance Management
* /AEB/CL_WU_PB_SPBT_CONTEXTS for functions of Origin & Preferences
