---
title: Preferential data for documents
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
  pages:
    - type: basic
      slug: process-description
      title: Process description
    - type: basic
      slug: >-
        example-for-method-aebcl_wu_pb_pref_chk_inv_bc-determine_preference_texts
      title: Implementation examples
---
You can make an origin declaration (also known as an ‘invoice declaration’ or ‘statement on origin’) on a commercial document that has enough detail in it to identify the origin of the goods. You can claim preference for different goods on the same document. You’ll need to clearly identify and show the goods that are originating and non-originating.\
In order to support the printout of the required information, we provide you public functions to retrieve the preferential data, especially the declaration text. The text is structured as one text on header and multiple texts on item level. If the preference information of an item corresponds to the header text, then the item text of this item is left empty.
