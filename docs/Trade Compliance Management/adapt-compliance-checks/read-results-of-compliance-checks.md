---
title: Use of check results in your own implementations
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: read-results-of-export-controls
      title: Read data from Export Controls and Risk Assessment
---
To handle the result of a compliance check in your own implementations, you can use the following function modules to read entries from the Compliance Monitor:

| Business object          | Read whole entry                    | Check if status is open      |
| :----------------------- | :---------------------------------- | :--------------------------- |
| Accounting document      | /AEB/CMP\_PB\_GET\_AD\_MON\_ENTRY   |                              |
| Applicant                | /AEB/CMP\_PB\_GET\_AP\_MON\_ENTRY   | /AEB/CMP\_PB\_IS\_AP\_OPEN   |
| Bank                     | /AEB/CMP\_PB\_GET\_BNK\_MON\_ENTRY  | /AEB/CMP\_PB\_IS\_BNK\_OPEN  |
| Business partner         | /AEB/CMP\_PB\_GET\_BP\_MON\_ENTRY   | /AEB/CMP\_PB\_IS\_BP\_OPEN   |
| Customer                 | /AEB/CMP\_PB\_GET\_CUS\_MON\_ENTRY  | /AEB/CMP\_PB\_IS\_CUS\_OPEN  |
| Delivery                 | /AEB/CMP\_PB\_GET\_DLV\_MON\_ENTRY  | /AEB/CMP\_PB\_IS\_DLV\_OPEN  |
| Employee                 | /AEB/CMP\_PB\_GET\_EM\_MON\_ENTRY   | /AEB/CMP\_PB\_IS\_EM\_OPEN   |
| Material document        | /AEB/CMP\_PB\_GET\_MD\_MON\_ENTRY   | /AEB/CMP\_PB\_IS\_MD\_OPEN   |
| Purchase document        | /AEB/CMP\_PB\_GET\_PD\_MON\_ENTRY   | /AEB/CMP\_PB\_IS\_PD\_OPEN   |
| Payment                  | /AEB/CMP\_PB\_GET\_PIP\_MON\_ENTRY  |                              |
| Sales document           | /AEB/CMP\_PB\_GET\_SDOC\_MON\_ENTRY | /AEB/CMP\_PB\_IS\_SDOC\_OPEN |
| Service notification     | /AEB/CMP\_PB\_GET\_SN\_MON\_ENTRY   | /AEB/CMP\_PB\_IS\_SN\_OPEN   |
| Service order            | /AEB/CMP\_PB\_GET\_SO\_MON\_ENTRY   | /AEB/CMP\_PB\_IS\_SO\_OPEN   |
| Vendor                   | /AEB/CMP\_PB\_GET\_VDR\_MON\_ENTRY  | /AEB/CMP\_PB\_IS\_VDR\_OPEN  |
| CRM business transaction | /AEB/CMP\_PB\_GET\_CBT\_MON\_ENTRY  |                              |
| CRM business partner     | /AEB/CMP\_PB\_GET\_CBP\_MON\_ENTRY  |                              |

Based on the found entries, you can decide on furher actions. Keep in mind that you can have mutliple entries per document because of different organization units, e.g. when the org unit is determined per plant and the plants are different for the document items.       

### How to determine a document is not relevant for a check?

When using one of the functions above, the business object might be not relevant for the Compliance checks due to the configuration. This can be recognized as follows:

* the export parameter EX\_MONITOR\_ENTRIES is initial
* the exception NO\_ORG\_UNIT\_RULE\_FOUND or NO\_ORG\_UNIT\_FOUND occurs

## Calling the functions when saving a sales order or delivery

For  sales orders and deliveries you can call  the according functions during the save in the user exit. If you execute the call in the prepare\_document-section of the user exit, the result is based on the monitor entry that was created before the current check (the check that is just executed in the save process). If  you call the function in the "Save\_Document"-section of the user exit, and after the /AEB/  include, the result is based on the current check.

## React to the check result of purchase documents

Purchase documents don't have any blocks. Therefore you might want to establish some individual reactions to a critical check result to prevent certain further processing.

> 🚧 The following suggestions rely on the individual setup and basis configuration of your SAP system.  Therefore, they might not work in each and every scenario and cannot be supported by AEB.

### Output messages

In case of a critical result, you might want to prevent the triggering of output messages, e.g. an order confirmation to the vendor. Assuming the dispatch time of the output is not set to “Send immediately (when saving the application)” one can define conditions in the procedure for application "EF".    There you could amend or define a routine for the field "Requirement" and use the function /AEB/CMP\_PB\_GET\_PD\_MON\_ENTRY to read the Compliance Monitor entry for the purchase order. The routine could check if one of found monitor entries  contains an “X” in the fields "HAS\_SCR\_HIT", "HAS\_SCR\_ERR", "HAS\_EC\_HIT" or "HAS\_EC\_ERR".   Or if the field "STATUS\_ID" is equal to "J" (Prohibited). Define a negative result in the requirement routine so it prevents the output message from being triggered.\
Once a formerly critical purchase order gets released in the Compliance Monitor, you can automatically trigger actions.   Implement the BAdI /AEB/CMP\_MONITOR\_11 , method PURCHASE\_DOCUMENT\_DEBLOCKED to react to the release in the monitor.

### Release of a purchase order using ME29N

SAP provides the standard functionality to put new purchase orders in a release process.  One could implement the BAdI ME\_PROCESS\_PO\_CUST => method CHECK to trigger the AEB Compliance check at the transaction for releasing these purchase orders (transaction code ME29N). 

> 🚧 SAP does not recommend to execute a COMMIT\_WORK in this BAdI. Calling an AEB function that triggers  a Compliance check will do so. This code example is just reading the result and not triggering a check. However,  make sure you're checking the potential technical risks.

```text Prevent release in ME29N
DATA:
  lcl_mepoheader  TYPE mepoheader,
  lcl_mon_entries TYPE /aeb/cmp_pb_entry_dos,
  lcl_mon_entry   TYPE /aeb/cmp_pb_entry_do,
  lcl_ekko        TYPE ekko.

IF sy-tcode <> 'ME29N'.
  RETURN.
ENDIF.

* ---------------------------------------------------
* Check AEB compliance screening status
* ---------------------------------------------------

* Determine current status (monitor entries) from AEB
lcl_mepoheader = im_header->get_data( ).
SELECT SINGLE * FROM ekko INTO lcl_ekko WHERE ebeln = lcl_mepoheader-ebeln.
CALL FUNCTION '/AEB/CMP_PB_GET_PD_MON_ENTRY'
  EXPORTING
    im_ekko            = lcl_ekko
  IMPORTING
    ex_monitor_entries = lcl_mon_entries
  EXCEPTIONS
    OTHERS             = 1.

IF sy-subrc <> 0.
  MESSAGE ID 'ZM' TYPE 'E' NUMBER '136'.
  RETURN.
ENDIF.

* There can be several entries for one document
LOOP AT lcl_mon_entries INTO lcl_mon_entry.
*   First check if the entry is still valid
  IF lcl_mon_entry-is_deleted = 'X' .
    CONTINUE.
  ENDIF.
*   Check and react if the status is "Critical
  IF lcl_mon_entry-has_scr_hit = 'X'.
    MESSAGE ID 'ZM' TYPE 'E' NUMBER 'XXX'.
*   Check and react if the status is "Error"
  ELSEIF lcl_mon_entry-has_scr_err = 'X'.
    MESSAGE ID 'ZM' TYPE 'E' NUMBER 'XXX'.
*   Check and react if the status is "Prohibited"
  ELSEIF lcl_mon_entry-status_id = 'J'.
    MESSAGE ID 'ZM' TYPE 'E' NUMBER 'XXX'.
  ENDIF.
ENDLOOP.
* ---------------------------------------------------
* End of AEB Check
* ---------------------------------------------------
```
