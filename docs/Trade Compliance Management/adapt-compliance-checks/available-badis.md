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
A common use case for custom developments is to perform certain actions depending on the result of a Compliance check. The add-on provides multiple BAdI methods, which are called on certain times and depending on the result.  
Each business object has a BAdI method to react to a certain result of the compliance check:

- The business object is critical. A match on a sanctioned party list was found or the document is subject to export restrictions.
- A technical error occurred. A check can not be done, because the connection to Trade Compliance Management is not working.
- The business object is not critical.  
  Additionally the business objects customer, vendor, business partner and service order also have a BAdI method, which is called if a critical business object changed to not critical anymore (for example when a good guy was defined).

## React to check results - available BAdIs

| Business object          | BAdI name            | Call time                                                                                                                      |
| :----------------------- | :------------------- | :----------------------------------------------------------------------------------------------------------------------------- |
| Customer                 | /AEB/CMP_CUSTOMER_02 | Called during online check                                                                                                     |
| Customer                 | /AEB/CMP_CUSTOMER_03 | Called during batch, monitor and asynchronous check                                                                            |
| Vendor                   | /AEB/CMP_VENDOR_02   | Called during online check                                                                                                     |
| Vendor                   | /AEB/CMP_VENDOR_03   | Called during batch, monitor and asynchronous check                                                                            |
| Sales document           | /AEB/CMP_SALES_DC_02 | Called during online check                                                                                                     |
| Sales document           | /AEB/CMP_SALES_DC_03 | Called during batch, monitor and asynchronous check                                                                            |
| Sales document           | /AEB/CMP_MONITOR_09  | Called from AEB Monitor: Sales document deblocked                                                                              |
| Delivery                 | /AEB/CMP_DLV_DC_02   | Called during online check                                                                                                     |
| Delivery                 | /AEB/CMP_DLV_DC_05   | Called during batch, monitor and asynchronous check                                                                            |
| Delivery                 | /AEB/CMP_MONITOR_01  | Called from AEB Monitor: Delivery deblocked                                                                                    |
| Purchase document        | /AEB/CMP_PURCH_DC_02 | Called during online, batch, monitor and asynchronous check                                                                    |
| Purchase document        | /AEB/CMP_MONITOR_11  | Called from AEB Monitor: Purchase document deblocked                                                                                           |
| Material document        | /AEB/CMP_MAT_DC_02   | Called during online, batch, monitor and asynchronous check                                                                    |
| Accounting document      | /AEB/CMP_ACC_DC_03   | Called during batch, monitor and asynchronous check                                                                            |
| Bank                     | /AEB/CMP_BANK_03     | Called during batch, monitor and asynchronous check                                                                            |
| Applicant                | /AEB/CMP_APPLICAN_03 | Called during batch, monitor and asynchronous check                                                                            |
| Employee                 | /AEB/CMP_EMPLOYEE_03 | Called during batch, monitor and asynchronous check                                                                            |
| Service order            | /AEB/CMP_SERV_ORD_02 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service order            | /AEB/CMP_SERV_ORD_03 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service order            | /AEB/CMP_SERV_ORD_04 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service order            | /AEB/CMP_SERV_ORD_06 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service notification     | /AEB/CMP_SERV_NOT_02 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service notification     | /AEB/CMP_SERV_NOT_03 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service notification     | /AEB/CMP_SERV_NOT_04 | Called during online, batch, monitor and asynchronous check                                                                    |
| Business partner         | /AEB/CMP_BUS_PRT_02  | Called during online, batch, monitor and asynchronous check if suspicious business partner found                               |
| Business partner         | /AEB/CMP_BUS_PRT_03  | Called during online, batch, monitor and asynchronous check if no suspicious business partner found                            |
| Business partner         | /AEB/CMP_BUS_PRT_04  | Called during online, batch, monitor and asynchronous check if an error in the result handling of the business partner occured |
| Business partner         | /AEB/CMP_BUS_PRT_07  | Called during online, batch, monitor and asynchronous check if a business partner is no longer suspicious                      |
| CRM business transaction | /AEB/CMP_CRM_BT_01   | Called during online, batch, monitor and asynchronous check                                                                    |
| CRM business transaction | /AEB/CMP_CRM_BT_02   | Called during online, batch, monitor and asynchronous check                                                                    |
| CRM business transaction | /AEB/CMP_CRM_BT_03   | Called during online, batch, monitor and asynchronous check                                                                    |
| CRM business transaction | /AEB/CMP_CRM_BT_04   | Called during online, batch, monitor and asynchronous check                                                                    |
| Service transaction (S4) | /AE1/CMP_SERV_TRA_02 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service transaction (S4) | /AE1/CMP_SERV_TRA_03 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service transaction (S4) | /AE1/CMP_SERV_TRA_04 | Called during online, batch, monitor and asynchronous check                                                                    |
| Service transaction (S4) | /AE1/CMP_SERV_TRA_05 | Called during online, batch, monitor and asynchronous check                                                                    |