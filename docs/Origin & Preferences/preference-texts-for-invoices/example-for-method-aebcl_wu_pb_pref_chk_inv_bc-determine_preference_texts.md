---
title: Implementation examples
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
## Method /AEB/CL_WU_PB_PREF_CHK_INV_BC - determine_preference_texts

This is an example for a call to retrieve the preference texts and how the output of the preference texts can be handled.

```text Preference text example
DATA:
  vbeln            TYPE vbeln_vf,
  org_unit         TYPE /aeb/wu_pb_org_unit,
  pref_chk_inv_bc  TYPE REF TO /aeb/cl_wu_pb_pref_chk_inv_bc,
  preference_texts TYPE /aeb/wu_pb_pref_text_result_do.
FIELD-SYMBOLS:
  <head_text_line> TYPE /aeb/wu_pb_text_line,
  <item_text>      TYPE /aeb/wu_pb_item_pref_text.

vbeln = <my vbeln>.

TRY.
    pref_chk_inv_bc = /aeb/cl_wu_pb_pref_chk_inv_bc=>new_for( im_vbrk_vbeln = vbeln ).
    preference_texts = pref_chk_inv_bc->determine_preference_texts( ).

    LOOP AT preference_texts-head_text ASSIGNING <head_text_line>.
      WRITE / <head_text_line>.
    ENDLOOP.

    SKIP 2.

    LOOP AT preference_texts-items_texts ASSIGNING <item_text>.
      WRITE / <item_text>-item_number.
      WRITE <item_text>-text_as_str.
    ENDLOOP.

  CATCH /aeb/cx_01_pb_missing_parm_sc.
*   add appropriate error handling.
	CATCH /aeb/cx_01_pb_bf_inv_sc.
*   add appropriate error handling.  
ENDTRY.
```

# Output

This is the output for an invoice with three items. In this example, the item 000010 has preference. The other two items have no preference.

![](https://files.readme.io/cc501fd-preference_text_example_output.png "preference_text_example_output.png")
