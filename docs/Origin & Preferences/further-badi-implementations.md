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
| BAdI name            | Description                                                                                                                                                                                                                                                      |
| :------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| /AEB/WU_MMDS_01      | Synchronization material data<br />This BAdI handles additional events during the synchronization of the material data. This BAdI runs per material and destination country during the journal synchronization (report: /AEB/01_JE_WORKER or /AEB/WU_JE_WORKER). |
| /AEB/WU_PRFS_01      | Synchronize Proof data<br />This BAdI runs during the journal synchronization (report: /AEB/01_JE_WORKER or /AEB/WU_JE_WORKER).                                                                                                                                  |
| /AEB/WU_DOC_CUS_01   | Data change any object<br />This BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.                                                                                                            |
| /AEB/WU_DOC_INV_01   | Data change invoice<br />This BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.                                                                                                               |
| /AEB/WU_DOC_SD_01    | Data change order<br />This BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.                                                                                                                 |
| /AEB/WU_DOC_DLV_01   | Data change delivery<br />This BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.                                                                                                              |
| /AEB/01_QTSK_CTRL_01 | Limiting the processes to be used by QCTSK<br />This BAdI offers the possibility to limit the used processes from server groups for QCTSK.                                                                                                                       |
| /AEB/01_SHM_01       | Activation of shared memory for texts                                                                                                                                                                                                                            |

## BAdI /AEB/01\_QTSK\_CTRL\_01

The parameter of the BAdI is a  runtime context object. It offers the following methods:

* GET\_RUNTIME\_CONTEXT (Provides the runtime context from which the task was created)
* GET\_USED\_SERVER\_GROUP\_NAME (Provides the used server group)
* GET\_SERVER\_GROUP\_MAX\_WPS\_COUNT (Provides the maximum number of processes of the server group)
* GET\_MAX\_WPS\_TO\_USE\_COUNT (Provides the maximum number of processes to be used)
* GET\_INITIAL\_FREE\_WPS\_COUNT (Provides the number of free processes at initialization)
* SET\_MAX\_WPS\_TO\_USE\_COUNT (Sets the maximum number of processes to be used)

The runtime context is used to define the number of processes to be used per use case. At the moment, the following possible runtime context exist:

* /aeb/cl\_wu\_pb\_spbt\_contexts=>c\_wu\_export (/AEB/WU\_EXPORT program)