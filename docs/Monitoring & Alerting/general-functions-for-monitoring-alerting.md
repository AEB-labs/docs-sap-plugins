---
title: General functions for Monitoring& Alerting
deprecated: false
hidden: false
metadata:
  robots: index
---
# Restrict background task usage

BAdI /AEB/01_QTSK_CTRL_01

This BAdI offers the possibility to limit the used processes from server groups for QCTSK. The method&#x20;

CHANGE_SPBT_RUNTIME_PARMS can be used.&#x20;

Parameters:

- IM_SPBT_RUNTIME_PARMS – context object for runtime<br />

Methods of the context object:

- GET_RUNTIME_CONTEXT (Provides the runtime context from which the task was created)
- GET_USED_SERVER_GROUP_NAME (Provides the used server group)
- GET_SERVER_GROUP_MAX_WPS_COUNT (Provides the maximum number of processes of the server group)
- GET_MAX_WPS_TO_USE_COUNT (Provides the maximum number of processes to be used)
- GET_INITIAL_FREE_WPS_COUNT (Provides the number of free processes at initialization)
- SET_MAX_WPS_TO_USE_COUNT (Sets the maximum number of processes to be used)

The runtime context is used to define the number of processes to be used per use case. At the moment, the following possible runtime context exist:

/aeb/cl_01_pb_spbt_contexts=>c_01_fcc_retry_job (Programm /AEB/01_FCC_RETRY_JOB)

<br />

# Activation of shared memory for texts

Use BAdI /AEB/01_SHM_01 for the activation of shared memory for texts.&#x20;

<br />
