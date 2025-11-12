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
<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        BAdI name
      </th>

      <th style={{ textAlign: "left" }}>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/WU\_MMDS\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Synchronization material data\
        This BAdI handles additional events during the synchronization of the material data. This BAdI runs per material and destination country during the journal synchronization (report: /AEB/01\_JE\_WORKER or /AEB/WU\_JE\_WORKER).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/WU\_DOC\_CUS\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Data change any object\
        This BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/WU\_DOC\_INV\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Data change invoice\
        This BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/WU\_DOC\_SD\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Data change order\
        This BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/WU\_DOC\_DLV\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Data change delivery\
        This BAdI makes it possible to change data after is has been populated by default to determine the declaration of origin.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/01\_QTSK\_CTRL\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Limiting the processes to be used by QCTSK\
        This BAdI offers the possibility to limit the used processes from server groups for QCTSK.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        /AEB/01\_SHM\_01
      </td>

      <td style={{ textAlign: "left" }}>
        Activation of shared memory for texts
      </td>
    </tr>
  </tbody>
</Table>

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
