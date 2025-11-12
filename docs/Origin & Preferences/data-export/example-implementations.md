---
title: Example implementations
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
## Change object data of a material

```text Change factory type
IF im_mara-mtart = 'ROH'.
  ch_material_do-factory_type = 'v'. "Merchandise (Fremdbeschafft)
ELSEIF im_mara-mtart = 'HALB'.
  ch_material_do-factory_type = 'i'. "Inhouse production (Eigenfertigung)
ELSE.
  ch_material_do-factory_type = 'x'. "Mixed origin (Mischbezug)
ENDIF.
```
```text Set material inactive and deleted
* Transfer the material as inactive and deleted if the plant-specific material 
* status has a certain value.

DATA:
  lv_matnr type matnr,
  ls_marc type marc.

lv_matnr = ch_material_do-material_no.

CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT' "Convert to internal format
        EXPORTING
          input  = lv_matnr
        IMPORTING
          output = lv_matnr.

IF im_marc-mmsta = <your special plant-specific material status>.
  ch_material_do-is_deleted = 'X'.
  ch_material_do-is_active = '-'.
ENDIF.
```

## Change object data of a customer

```text Change adress data
* The following contact person data from SAP in the partner role <partner role> 
* should be transferred for customers and vendors specifically for requesting 
* and issuing proofs according to O&P
* If partner function <partner role> is not created or fields there are empty, 
* the "General Data" should be transferred. 

DATA:
      lv_namecomplete TYPE c LENGTH 35,
      ls_knvp         TYPE knvp,
      ls_knvk         TYPE knvk,
      ls_adr6         TYPE adr6.
      
SELECT SINGLE * FROM knvp INTO ls_knvp 
       WHERE kunnr = im_kna1-kunnr AND parvw = '<partner role>'.

    IF sy-subrc = 0.
      SELECT SINGLE * FROM knvk INTO ls_knvk WHERE parnr = ls_knvp-parnr.
      SELECT  SINGLE * FROM adr6 INTO ls_adr6 WHERE persnumber = ls_knvk-prsnr.

      CONCATENATE ls_knvk-namev ls_knvk-name1 INTO lv_namecomplete SEPARATED BY ' '.
      ch_address_do-contact_person = lv_namecomplete.
      ch_address_do-contact_person_title = ls_knvk-anred.
      ch_address_do-tel_no = ls_knvk-telf1.
      ch_address_do-department = ls_knvk-abtpa.
      ch_address_do-email = ls_adr6-smtp_addr.
    ENDIF.
```