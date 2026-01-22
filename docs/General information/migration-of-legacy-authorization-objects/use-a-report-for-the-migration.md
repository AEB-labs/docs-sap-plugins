---
title: Check used authorization objects
deprecated: false
hidden: false
metadata:
  robots: index
---
This report checks the AEB authorization objects used in your SAP system. 

As a result, it generates a detailed output informing you about the next steps.
In case of transferring authorization objects is needed, please send the detailed output to the AEB support in order to get the required transport requests. Importing the transport requests will move the concerned authorization objects into the package ZAEB_DEPRECATED_OBJECTS.

Copy the following code block into a report and execute it in your quality assurance system. The output of the report will inform you about the next steps.

```Text Report code
CONSTANTS:
  c_basis_cockpit   TYPE xuobject VALUE 'ZAEB_01C10',
  c_cmp_monitor     TYPE xuobject VALUE 'ZAEB_CMP32',
  c_aes_monitor     TYPE xuobject VALUE 'ZAEB_AES06',
  c_emcs_logentries TYPE xuobject VALUE 'ZAEB_ELE01',
  c_ma_desktop      TYPE xuobject VALUE 'ZAEB_MAC01',
  c_pa_desktop      TYPE xuobject VALUE 'ZAEB_PA001',
  c_ta_desktop      TYPE xuobject VALUE 'ZAEB_TA072',
  c_wu_mat_monitor  TYPE xuobject VALUE 'ZAEB_WUM01',

  c_basis_package   TYPE devclass VALUE 'ZAEB_01_NAMESPACELESS_OBJECTS',
  c_cmp_package     TYPE devclass VALUE 'ZAEB_CMP_NAMESPACELESS_OBJECTS',
  c_aes_package     TYPE devclass VALUE 'ZAEB_AES_NAMESPACELESS_OBJECTS',
  c_emcs_package    TYPE devclass VALUE 'ZAEB_EM_NAMESPACELESS_OBJECTS',
  c_ma_package      TYPE devclass VALUE 'ZAEB_MA_NAMESPACELESS_OBJECTS',
  c_pa_package      TYPE devclass VALUE 'ZAEB_PA_NAMESPACELESS_OBJECTS',
  c_ta_package      TYPE devclass VALUE 'ZAEB_TA_NAMESPACELESS_OBJECTS',
  c_wu_package      TYPE devclass VALUE 'ZAEB_WU_NAMESPACELESS_OBJECTS'.

DATA:
  BEGIN OF auth_objects_found,
    basis TYPE char1,
    cmp   TYPE char1,
    aes   TYPE char1,
    emcs  TYPE char1,
    ma    TYPE char1,
    pa    TYPE char1,
    ta    TYPE char1,
    wu    TYPE char1,
  END OF auth_objects_found.

PERFORM query_auth_object USING c_basis_cockpit c_basis_package CHANGING auth_objects_found-basis.
PERFORM query_auth_object USING c_cmp_monitor c_cmp_package CHANGING auth_objects_found-cmp.
PERFORM query_auth_object USING c_aes_monitor c_aes_package CHANGING auth_objects_found-aes.
PERFORM query_auth_object USING c_emcs_logentries c_emcs_package CHANGING auth_objects_found-emcs.
PERFORM query_auth_object USING c_ma_desktop c_ma_package CHANGING auth_objects_found-ma.
PERFORM query_auth_object USING c_pa_desktop c_pa_package CHANGING auth_objects_found-pa.
PERFORM query_auth_object USING c_ta_desktop c_ta_package CHANGING auth_objects_found-ta.
PERFORM query_auth_object USING c_wu_mat_monitor c_wu_package CHANGING auth_objects_found-wu.

PERFORM output_result.

FORM query_auth_object USING auth_object TYPE xuobject package TYPE devclass CHANGING is_used TYPE char1.
  DATA:
    tmp_tadir    TYPE tadir,
    tmp_agr_1250 TYPE agr_1250.
  is_used = '-'.

*  1. Check if auth object is in the default ZAEB package.
*  2. Check if it is used in an auth profile.
  SELECT SINGLE * FROM tadir INTO tmp_tadir WHERE obj_name = auth_object AND devclass = package.
  IF sy-subrc = 0.
    SELECT SINGLE * FROM agr_1250 INTO tmp_agr_1250 WHERE object = auth_object.
    IF sy-subrc = 0.
      is_used = 'X'.
    ENDIF.
  ENDIF.
ENDFORM.

FORM output_result.
  SKIP.
  WRITE: 'System:', sy-sysid.
  SKIP.

  IF auth_objects_found-basis <> 'X'
    AND auth_objects_found-cmp <> 'X'
    AND auth_objects_found-aes <> 'X'
    AND auth_objects_found-emcs <> 'X'
    AND auth_objects_found-ma <> 'X'
    AND auth_objects_found-pa <> 'X'
    AND auth_objects_found-ta <> 'X'
    AND auth_objects_found-wu <> 'X'.
    WRITE 'There are no authorization objects to migrate.'.
    RETURN.
  ENDIF.

  WRITE: 'You have to migrate the authorization objects of the following products:'.
  SKIP.

  IF auth_objects_found-basis = 'X'
    AND auth_objects_found-cmp = 'X'
    AND auth_objects_found-aes = 'X'
    AND auth_objects_found-emcs = 'X'
    AND auth_objects_found-ma = 'X'
    AND auth_objects_found-pa = 'X'
    AND auth_objects_found-ta = 'X'
    AND auth_objects_found-wu = 'X'.
    WRITE /4 'All products'.
  ELSE.
    IF auth_objects_found-basis = 'X'.
      WRITE /4 'Basis'.
    ENDIF.
    IF auth_objects_found-cmp = 'X'.
      WRITE /4 'Trade Compliance Management'.
    ENDIF.
    IF auth_objects_found-aes = 'X'.
      WRITE /4 'Customs Management'.
    ENDIF.
    IF auth_objects_found-emcs = 'X'.
      WRITE /4 'EMCS Filing'.
    ENDIF.
    IF auth_objects_found-ma = 'X'.
      WRITE /4 'Monitoring & Alerting / CES'.
    ENDIF.
    IF auth_objects_found-pa = 'X'.
      WRITE /4 'Carrier Connect'.
    ENDIF.
    IF auth_objects_found-ta = 'X'.
      WRITE /4 'Product Classification'.
    ENDIF.
    IF auth_objects_found-wu = 'X'.
      WRITE /4 'Origin & Preferences'.
    ENDIF.
  ENDIF.

  SKIP.
  WRITE /'Contact AEB Support to get the required transport requests.'.
ENDFORM.
```

<br />
