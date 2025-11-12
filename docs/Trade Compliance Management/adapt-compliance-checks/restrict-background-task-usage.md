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
[block:api-header]
{
  "title": "Scenario"
}
[/block]
When you want to use background tasks to check multiple business objects parallel you might overload the connected engine, because the SAP system is sending to many requests per second. To limit how many requests are sent you can either resize the logon group /AEB/DEFAULT or implement the BAdI /AEB/01_QTSK_CTRL_01. Resizing the logon group is easier but also not as exact, because you can only specify a percentage of processes which could be used. The BAdI on the other hand allows you to specify the exact amount of processes to be used and even allows you to change it depending on the use case.
[block:api-header]
{
  "title": "BAdI /AEB/01_QTSK_CTRL_01"
}
[/block]
The parameter of the BAdI is a  runtime context object. It offers the following methods:
- GET_RUNTIME_CONTEXT (Provides the runtime context from which the task was created)
- GET_USED_SERVER_GROUP_NAME (Provides the used server group)
- GET_SERVER_GROUP_MAX_WPS_COUNT (Provides the maximum number of processes of the server group)
- GET_MAX_WPS_TO_USE_COUNT (Provides the maximum number of processes to be used)
- GET_INITIAL_FREE_WPS_COUNT (Provides the number of free processes at initialization)
- SET_MAX_WPS_TO_USE_COUNT (Sets the maximum number of processes to be used)

You can simply set the maximum number of processes to the amount that should be used.
[block:code]
{
  "codes": [
    {
      "code": "METHOD /aeb/if_ex_01_qtsk_ctrl_01~change_spbt_runtime_parms.\n  IF im_spbt_runtime_parms->get_runtime_context( ) = /aeb/cl_cmp_pb_spbt_contexts=>c_cmp_bank_check. \"If the bank check is run\n    im_spbt_runtime_parms->set_max_wps_to_use_count( 2 ). \"Limit processes to 2\n  ENDIF.\nENDMETHOD.",
      "language": "text",
      "name": "Limit bank check"
    }
  ]
}
[/block]