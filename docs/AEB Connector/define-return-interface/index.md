---
title: Synchronize data between ASSIST4 and SAP
excerpt: How to implement the BAdI /AEB/CT_NSG_WRK_01
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: define-the-custom-structure
      title: Define the custom structure
    - type: basic
      slug: realize-basic-custom-call
      title: Realize basic custom call
    - type: basic
      slug: handle-mrn
      title: Handle MRN
---
The AEB Connector uses the publish-subscribe pattern to realize a synchronization between the ASSIST4 system and the SAP system. In the ASSIST4 system you can define which updates trigger a message. This message is then saved and SAP uses a job (/AEB/01\_JE\_WORKER) to regularly check if there are relevant messages. These messages then are sent to the BAdI /AEB/CT\_NSG\_WRK\_01 to be handled. The BAdI then can get custom data from the ASSIST4 system to further process the message. This has the advantage that the interface can be specifically tailored to your wishes. Also no proxy is needed to connect the ASSIST4 system with the SAP system, because all requests are done synchronously by the SAP system and not from the ASSIST4 system.
