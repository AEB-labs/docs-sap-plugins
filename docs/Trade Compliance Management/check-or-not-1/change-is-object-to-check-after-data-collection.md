---
title: Disable Export control checks (after data collection)
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
      slug: change-data-of-export-controls-check
      title: Change the data for Export Controls checks
---
The BAdI /AEB/CMP_EC_CHK_01 is called after all data for Export Control has been collected by the standard logic. The collected data is provided as a read-only object. 

Within the BAdI you can decide on wether or not the business object should be checked ("no check logic"). This BAdI will run for every business object. The configuration setting for checks of domestic documents is also considered. 

This example outlines how to exclude documents from a check that are shipping inside the EU and have no export classifications:

```Text EU-only documents with no export classifications
    IF ch_is_to_check <> 'X'.
      RETURN.
    ENDIF.

    LOOP AT im_value->get_items( ) INTO DATA(item).

      LOOP AT item->get_product_classifications( ) INTO DATA(pcls).
        IF pcls->get_classification_identcode( ) = 'ClassificationAusfuhrliste'.
          IF pcls->get_classification_number( ) <> 'NOT_LISTED'.
            RETURN.
          ENDIF.
        ENDIF.
        IF pcls->get_classification_identcode( ) = 'CLASSIFICATION_US_EAR_CCL'.
          IF pcls->get_classification_number( ) <> 'NOT_LISTED'.
            RETURN.
          ENDIF.
        ENDIF.
      ENDLOOP.

      LOOP AT item->get_partners( ) INTO DATA(partner).
        IF NOT is_in_eu( partner ).
          RETURN.
        ENDIF.
      ENDLOOP.

    ENDLOOP.
    ch_is_to_check = '-'.

```

Please note that the "is_in_eu"-method must be defined by yourself, containing the required logic.