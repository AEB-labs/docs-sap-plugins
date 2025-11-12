---
title: Reacting to overall check result
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
      slug: add-data-to-compliance-monitor
      title: Additional data for Compliance Monitor
---
A common use case for custom developments is to perform certain actions depending on the result of a Compliance check. The add-on provides multiple BAdI methods, which are called on certain times and depending on the result.\
Each business object has a BAdI method to react to a certain result of the compliance check:

* The business object is critical. A match on a sanctioned party list was found or the document is subject to export restrictions.
* A technical error occurred. A check can not be done, because the connection to Trade Compliance Management is not working.
* The business object is not critical.\
  Additionally the business objects customer, vendor, business partner and service order also have a BAdI method, which is called if a critical business object changed to not critical anymore (for example when a good guy was defined).

## React to check results - available BAdIs

| Business object          | BAdI name               | Call time                                                                                                                      |
| :----------------------- | :---------------------- | :----------------------------------------------------------------------------------------------------------------------------- |
| Customer                 | /AEB/CMP\_CUSTOMER\_02  | Called during online check                                                                                                     |
| Customer                 | /AEB/CMP\_CUSTOMER\_03  | Called during batch, monitor and asynchronous check                                                                            |
| Vendor                   | /AEB/CMP\_VENDOR\_02    | Called during online check                                                                                                     |
| Vendor                   | /AEB/CMP\_VENDOR\_03    | Called during batch, monitor and asynchronous check                                                                            |
| Sales document           | /AEB/CMP\_SALES\_DC\_02 | Called during online check                                                                                                     |
| Sales document           | /AEB/CMP\_SALES\_DC\_03 | Called during batch, monitor and asynchronous check                                                                            |
| Sales document           | /AEB/CMP\_MONITOR\_09   | Called from AEB Monitor: Sales document deblocked                                                                              |
| Delivery                 | /AEB/CMP\_DLV\_DC\_02   | Called during online check                                                                                                     |
| Delivery                 | /AEB/CMP\_DLV\_DC\_05   | Called during batch, monitor and asynchronous check                                                                            |
| Delivery                 | /AEB/CMP\_MONITOR\_01   | Called from AEB Monitor: Delivery deblocked                                                                                    |
| Purchase document        | /AEB/CMP\_PURCH\_DC\_02 | Called during online, batch, monitor and asynchronous check                                                                    |
| Purchase document        | /AEB/CMP\_MONITOR\_11   | Called from AEB Monitor: Purchase document deblocked                                                                           |
| Material document        | /AEB/CMP\_MAT\_DC\_02   | Called during online, batch, monitor and asynchronous check                                                                    |
| Accounting document      | /AEB/CMP\_ACC\_DC\_03   | Called during batch, monitor and asynchronous check                                                                            |
| Bank                     | /AEB/CMP\_BANK\_03      | Called during batch, monitor and asynchronous check                                                                            |
| Applicant                | /AEB/CMP\_APPLICAN\_03  | Called during batch, monitor and asynchronous check                                                                            |
| Employee                 | /AEB/CMP\_EMPLOYEE\_03  | Called during batch, monitor and asynchronous check                                                                            |
| Service order            | /AEB/CMP\_SERV\_ORD\_02 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service order            | /AEB/CMP\_SERV\_ORD\_03 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service order            | /AEB/CMP\_SERV\_ORD\_04 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service order            | /AEB/CMP\_SERV\_ORD\_06 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service notification     | /AEB/CMP\_SERV\_NOT\_02 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service notification     | /AEB/CMP\_SERV\_NOT\_03 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service notification     | /AEB/CMP\_SERV\_NOT\_04 | Called during online, batch, monitor and asynchronous check                                                                    |
| Business partner         | /AEB/CMP\_BUS\_PRT\_02  | Called during online, batch, monitor and asynchronous check if suspicious business partner found                               |
| Business partner         | /AEB/CMP\_BUS\_PRT\_03  | Called during online, batch, monitor and asynchronous check if no suspicious business partner found                            |
| Business partner         | /AEB/CMP\_BUS\_PRT\_04  | Called during online, batch, monitor and asynchronous check if an error in the result handling of the business partner occured |
| Business partner         | /AEB/CMP\_BUS\_PRT\_07  | Called during online, batch, monitor and asynchronous check if a business partner is no longer suspicious                      |
| CRM business transaction | /AEB/CMP\_CRM\_BT\_01   | Called during online, batch, monitor and asynchronous check                                                                    |
| CRM business transaction | /AEB/CMP\_CRM\_BT\_02   | Called during online, batch, monitor and asynchronous check                                                                    |
| CRM business transaction | /AEB/CMP\_CRM\_BT\_03   | Called during online, batch, monitor and asynchronous check                                                                    |
| CRM business transaction | /AEB/CMP\_CRM\_BT\_04   | Called during online, batch, monitor and asynchronous check                                                                    |
| Service transaction (S4) | /AE1/CMP\_SERV\_TRA\_02 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service transaction (S4) | /AE1/CMP\_SERV\_TRA\_03 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service transaction (S4) | /AE1/CMP\_SERV\_TRA\_04 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service transaction (S4) | /AE1/CMP\_SERV\_TRA\_05 | Called during online, batch, monitor and asynchronous check                                                                    |
