---
title: Individual address check
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
The following functions are available to trigger the screening check in a flexible way. 

> 📘 Please note that these functions will not create any entries in the Compliance monitor entries nor logs .

If you would like to do a screening check for any kind of address (even if there is not an existing document / master data record in the system)  you can use one of the following function modules:\
(1) /AEB/CMP\_ADDR\_CHK\_FOR\_ADDR1VAL\
(2) /AEB/CMP\_ADDR\_CHK\_FOR\_ADDRNR\
(3) /AEB/CMP\_ADDR\_CHK\_FOR\_HANDLE

Each function module provides the following import parameters:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Field name
      </th>

      <th>
        Type
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        (1) IM\_ADDR1\_VAL
        (2) IM\_ADDRNR
        (3) IM\_ADDRHANDLE
      </td>

      <td>
        (1) ADDR1\_VAL\
        (2) ADDR1\_VAL-ADDRNUMBER\
        (3) ADDR1\_VAL-ADDRHANDLE
      </td>

      <td>
        (1) Structure with address values\
        (2) address number\
        (3) address handle
      </td>
    </tr>

    <tr>
      <td>
        IM\_SUPPRESS\_ENGINE\_LOGGING
      </td>

      <td>
        CHAR1
      </td>

      <td>
        Flag to supress logs
      </td>
    </tr>

    <tr>
      <td>
        IM\_RFC\_DESTINATION
      </td>

      <td>
        RFCDEST
      </td>

      <td>
        Defined destination to AEB engine
      </td>
    </tr>

    <tr>
      <td>
        IM\_ENGINE\_CLIENT
      </td>

      <td>
        /AEB/CMP\_ENGN\_CLIENT
      </td>

      <td>
        Client name from the AEB engine
      </td>
    </tr>

    <tr>
      <td>
        IM\_ENGINE\_PROFILE
      </td>

      <td>
        /AEB/CMP\_ENGN\_PROFILE
      </td>

      <td>
        Profile Name from the AEB engine
      </td>
    </tr>

    <tr>
      <td>
        IM\_REF\_COMMENT
      </td>

      <td>
        CHAR255
      </td>

      <td>
        Comment for the check – e.g. a document ID or master data ID  (optional)
      </td>
    </tr>

    <tr>
      <td>
        IM\_REF\_ID
      </td>

      <td>
        /AEB/CMP\_ENTRY\_REF\_ID
      </td>

      <td>
        An internal reference number (optional)
      </td>
    </tr>

    <tr>
      <td>
        IM\_ORG\_UNIT
      </td>

      <td>
        /AEB/CMP\_PB\_ORG\_UNIT
      </td>

      <td>
        If provided, used to retrieve engine client and profile. Replaces the corresponding import parameters.
      </td>
    </tr>

    <tr>
      <td>
        IM\_ADDRESS\_TYPE
      </td>

      <td>
        CHAR20
      </td>

      <td>
        Possible values are:\
        •	"individual"\
        •	"entity"\
        •	"meansOfTransport"\
        •	"unknown"
      </td>
    </tr>
  </tbody>
</Table>
