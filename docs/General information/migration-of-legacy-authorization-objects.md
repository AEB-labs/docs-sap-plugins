---
title: Migration of legacy authorization objects
deprecated: false
hidden: true
metadata:
  robots: index
---
<br />

In the past, AEB delivered authorization objects in the Z namespace. Depending on when and how you first installed AEB components in your SAP systems, you maintained these objects in authorization roles. AEB was informed about difficulties in S/4HANA migrations related to these authorization objects which could result in their deletion. You have the following options to prevent this:

1. Copy the following code block into a report and execute it in your system. The output of the report you will inform you about the next steps.

2. <Accordion title="Report code" icon="fa-info-circle">
   ```Text zaeb_check_auth_migration
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
   </Accordion>

3. Manually move the objects to another package.
   Move all objects The package name must begin with a Z, otherwise it is freely selectable. However, do not select any of the ZAEB_*_NAMESPACELESS_OBJECTS packages.
   Four objects have additionally maintained documentation, which is available as a separate entry in the package. However, they are not displayed in the SE80. The objects in the TADIR table are as follows:
   UOZAEB_AAW00
   UOZAEB_AEB00
   UOZAEB_AES00
   UOZAEB_AOU01
   This documentation has no function and should be deleted. To do this, log on to the system in English and start transaction SU21. Open the relevant authorization object (without the UO at the beginning). Now switch to change mode and click on "Create Object Documentation". On the next screen, execute the "Delete" function via the menu.

4. Switch to the new authorization objects. Each Z authorization object has been replaced by an authorization object in the /AEB/ namespace. In order for these to be used, the legacy Z authorization objects must be deleted from your system. All uses in roles must be removed before deletion, otherwise the roles will become invalid.
