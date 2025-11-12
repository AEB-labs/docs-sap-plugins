---
title: Read results of overall check (COPY)
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Use the following APIs to read entries from the Compliance Monitor. 

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

For  sales orders and deliveries you can call  the according functions during the save in the user exit. If you execute the call in the prepare\_document-section of the user exit, the result is based on the monitor entry that was created before the current check (the check that is just executed in the save process). If  you do the call in the save\_document-Section of the userexit , after the /AEB/  include, the result is based on the current check. 

## Reacting to a critical result of a purchase document check

Purchase document don't have standard blocks. Therefore you might want to establish some individual reactions to a critical check result and prevent further processing for the purchase order. 

<br />

Noch ergänzen: nicht supported, hängt von SAP Laufzeitumgebung ab, nicht 100% garantiert

### Prevent certain output messages

There are two different approaches, depending on the dispatch time of the messages.

1. When dispatch time is 'Send immediately (when saving the application)'
   1. Implement the customer exit for the purchase document check as described in the installation manual
   2. Implement all methods of BAdI /AEB/CMP\_PURCH\_DC\_02 to set a static variable, whether the check result is suspicious (SUSPICIOUS\_FOUND) or could not be performed due to an error (ERROR\_OCCURRED).
   3. After the AEB include is processed in the customer exit, read the static variable and decide whether to prevent the output message.
   4. In order to automatically trigger actions after the release of the purchase order, use the AEB BAdI "/AEB/CMP\_MONITOR\_11 – PURCHASE\_DOCUMENT\_DEBLOCKED" for your coding.
2. Other dispatch time settings
   1. Requirement: In case of a critical result, prevent triggering an output message, e.g. order confirmation to the vendor.
   2. To do so, you can define conditions in the procedure. Start transaction NACE, select the application "EF" and click on Procedures. Select the procedure,  and click on "control"-tree  at the left.
   3. Choose the desired output type, e.g. the type for the order  confirmation and amend or define a routine for the field "Requirement" .
   4. In this routine, use the AEB function /AEB/CMP\_PB\_GET\_PD\_MON\_ENTRY to read the Compliance Monitor entry.
   5. If one of the parameters "HAS\_SCR\_HIT", "HAS\_SCR\_ERR", "HAS\_EC\_HIT" or "HAS\_EC\_ERR" is equal to "X" or "STATUS\_ID" is equal to "J" (Prohibited) for one of the entries found, then prevent the outgoing message from being triggered
   6. In order to automatically trigger actions after the release of a purchase order, use the AEB BAdI "/AEB/CMP\_MONITOR\_11 – PURCHASE\_DOCUMENT\_DEBLOCKED" for your coding. 

### Prevent release of the order via ME29

Code sample for BadI "ME\_PROCESS\_PO\_CUST => Methode CHECK"  Noch ergänzen: COMMIT WORK, wird von SAP aber hier nicht empfohlen. Auf eigenes Risiko  

```text Prevent release
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
