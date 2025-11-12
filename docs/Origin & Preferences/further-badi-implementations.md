---
title: Further BAdI definitions
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
[block:parameters]
{
  "data": {
    "h-0": "BAdI name",
    "h-1": "Description",
    "0-0": "/AEB/WU_MMDS_01",
    "0-1": "Synchronization material data  \nThis BAdI handles additional events during the synchronization of the material data. This BAdI runs per material and destination country during the journal synchronization (report: /AEB/01_JE_WORKER or /AEB/WU_JE_WORKER).",
    "1-0": "/AEB/WU_DOC_CUS_01",
    "1-1": "Data change any object  \nThis BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.",
    "2-0": "/AEB/WU_DOC_INV_01",
    "2-1": "Data change invoice  \nThis BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.",
    "3-0": "/AEB/WU_DOC_SD_01",
    "3-1": "Data change order  \nThis BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.",
    "4-0": "/AEB/WU_DOC_DLV_01",
    "4-1": "Data change delivery  \nThis BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.",
    "5-0": "/AEB/01_QTSK_CTRL_01",
    "5-1": "Limiting the processes to be used by QCTSK  \nThis BAdI offers the possibility to limit the used processes from server groups for QCTSK.",
    "6-0": "/AEB/01_SHM_01",
    "6-1": "Activation of shared memory for texts"
  },
  "cols": 2,
  "rows": 7,
  "align": [
    "left",
    "left"
  ]
}
[/block]




## BAdI /AEB/01_QTSK_CTRL_01

The parameter of the BAdI is a  runtime context object. It offers the following methods:

- GET_RUNTIME_CONTEXT (Provides the runtime context from which the task was created)
- GET_USED_SERVER_GROUP_NAME (Provides the used server group)
- GET_SERVER_GROUP_MAX_WPS_COUNT (Provides the maximum number of processes of the server group)
- GET_MAX_WPS_TO_USE_COUNT (Provides the maximum number of processes to be used)
- GET_INITIAL_FREE_WPS_COUNT (Provides the number of free processes at initialization)
- SET_MAX_WPS_TO_USE_COUNT (Sets the maximum number of processes to be used)

The runtime context is used to define the number of processes to be used per use case. At the moment, the following possible runtime context exist:

- /aeb/cl_wu_pb_spbt_contexts=>c_wu_export (/AEB/WU_EXPORT program)