---
title: Advanced shipping
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## API class /AEB/CL\_PA\_PB\_CARRIER\_BF

To use the full API functionality of Carrier Cloud for SAP, you can use those classes: 

**/AEB/CL\_PA\_PB\_CARRIER\_BF**

 **/AEB/CL\_PA\_PB\_BSMCARRIER\_BF**

<Image alt="The API class provided by the add-on for SAP" align="center" src="https://files.readme.io/f8bbe8d86094fde2efeff1a377740e3a454cd6e3a100a7a92a316980de71d3a2-image.png" />

![](https://files.readme.io/1b62b7651a3e392592c4d24588c917ae14eb543734bfb6467dad4dcbae4b7447-image.png)

<br />

The API classes are provided by the add-on for SAP

This classes enables you to call every available method and also change the field values and parameters completely flexible. For a detailed documentation of this API see here: [API Documentation Carrier Cloud](https://transport-freight-management.docs.developers.aeb.com/docs/about-carrier-connect).

## Supporting functions

Before you can use any of the methods from the class, e.g. Create\_Shipment, certain mandatory data is needed. To support you gathering the required data, there are some complementary features, which will be explained in this section: 

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Task
      </th>

      <th>
        Class
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Determination of the org unit
      </td>

      <td>
        * \*TM document: \*\*/AE1/CL\_PA\_PB\_FRO\_OU\_RULE\_BC  
        * \*Delivery: \*\*/AEB/CL\_PA\_PB\_DLV\_OU\_RULE\_BC  
        * \*Shipment: \*\*/AEB/CL\_PA\_PB\_SHP\_OU\_RULE\_BC
      </td>
    </tr>

    <tr>
      <td>
        Determination of connection parms
      </td>

      <td>
        /AEB/CL\_PA\_PB\_ENGN\_PRM\_BC
      </td>
    </tr>

    <tr>
      <td>
        Collecting data from SAP business object ("Collector")\
        This function provides the basic data based on the settings in the configuration. It runs also through the "AFTER\_STD\_FILLING"-method of the according BAdI for each object.
      </td>

      <td>
        * \*TM document: \*\*/AE1/CL\_PA\_PB\_FRO\_SHP\_COLL\_BC  
        * \*Delivery: \*\*/AEB/CL\_PA\_PB\_DLV\_SHP\_COLL\_BC  
        * \*Shipment: \*\*/AEB/CL\_PA\_PB\_SHP\_SHP\_COLL\_BC
      </td>
    </tr>

    <tr>
      <td>
        Determination of the workstation ID
      </td>

      <td>
        /AEB/CL\_PA\_PB\_WSTA\_ID\_BC
      </td>
    </tr>

    <tr>
      <td>
        Determination of the reference number
      </td>

      <td>
        * \*TM document: \*\*/AE1/CL\_PA\_PB\_FRO\_SHP\_REF\_BC  
        * \*Delivery:\*\* /AEB/CL\_PA\_PB\_DLV\_SHP\_REF\_BC  
        * \*Shipment: \*\*/AEB/CL\_PA\_PB\_SHP\_SHP\_REF\_BC
      </td>
    </tr>
  </tbody>
</Table>

The names of the classes are structured like this: 

* /AEB/CL\_PA\_PB  - Class is for public usage in customer implementations of Carrier Cloud for SAP
* DLV , SHP, FRO  -  the SAP document the class is applicable for: delivery, shipment or freight order (aka TM document)
* OU\_RULE\_BC, SHP\_COLL\_BC, SHP\_REF\_BC, WSTA\_ID\_BC - the functionality the class can be used for: organizational unit rule (OU\_RULE), collect shipping data (SHIP\_COLL), get reference (REF) or get workstation data (WSTA).     

In this part of the guide we will walk through all those functions. All examples are based on an outbound delivery, but you can adapt them to other supported SAP business objects using the according class as listed above. Regardless of the SAP business object you're transferring, the result will be a "shipping order" in Carrier Cloud. 

## Organizational unit

First step is to determine the AEB specific organizational unit for a business object. The org unit is helpful to separate data access and read certain data from the configuration. This program determines the organizational unit for a delivery and writes it to the screen: 

```text Determine the org unit
REPORT zaeb_create_first_shipment.

PARAMETERS: vbeln TYPE likp-vbeln.

DATA:
  likp             TYPE likp,
  lipss            TYPE STANDARD TABLE OF lips,
  vbpas            TYPE STANDARD TABLE OF vbpa,
  vekps            TYPE STANDARD TABLE OF vekp,
	vepos            TYPE STANDARD TABLE OF vepo,
  org_unit_rule_bc TYPE REF TO /aeb/cl_pa_pb_dlv_ou_rule_bc,
  org_unit         TYPE string.

SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.
SELECT * FROM lips INTO TABLE lipss WHERE vbeln = vbeln.
SELECT * FROM vbpa INTO TABLE vbpas WHERE vbeln = vbeln.
"Determine vekps for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB
"Determine vepos for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB


org_unit_rule_bc = /aeb/cl_pa_pb_dlv_ou_rule_bc=>new_for(
		im_likp = likp 
    im_lipss = lipss ).
    
org_unit = org_unit_rule_bc->get_org_unit( ).

WRITE: 'OrgUnit: ' , org_unit.
WRITE /.
```

## Connection parameters

The next step is to read the connection parameters linked to this organizational unit. This connection is used to call the endpoint (the Carrier Cloud for SAP) eventually.  

```text Determine the connection parameters
REPORT zaeb_create_first_shipment.

PARAMETERS: vbeln TYPE likp-vbeln.

DATA:
  likp             TYPE likp,
  lipss            TYPE STANDARD TABLE OF lips,
  vbpas            TYPE STANDARD TABLE OF vbpa,
  vekps            TYPE STANDARD TABLE OF vekp,
	vepos            TYPE STANDARD TABLE OF vepo,
  org_unit_rule_bc TYPE REF TO /aeb/cl_pa_pb_dlv_ou_rule_bc,
  org_unit         TYPE /aeb/01_char20,
  engn_prm_bc      TYPE REF TO /aeb/cl_pa_pb_engn_prm_bc,
  engn_prm_mo      TYPE REF TO /aeb/if_pa_pb_engn_prm_mo.

SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.
SELECT * FROM lips INTO TABLE lipss WHERE vbeln = vbeln.
SELECT * FROM vbpa INTO TABLE vbpas WHERE vbeln = vbeln.
"Determine vekps for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB
"Determine vepos for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB

*Org unit is required to read configured values
org_unit_rule_bc = /aeb/cl_pa_pb_dlv_ou_rule_bc=>new_for( im_likp = likp im_lipss = lipss ).
org_unit = org_unit_rule_bc->get_org_unit( ).

engn_prm_bc = /aeb/cl_pa_pb_engn_prm_bc=>get_instance( ).
engn_prm_mo = engn_prm_bc->get_engn_prm_do_for( org_unit ).

WRITE: 'Destination: ' , engn_prm_mo->get_destination( ).
WRITE: 'Engine client: ' , engn_prm_mo->get_engine_client( ).s
```

If you run the report again the destination and client for Carrier Cloud for SAP will be shown. 

## Collecting data from an SAP business object

Next, we need the data for the shipping order that we want to send to Carrier Cloud. Use the "collector"-class for the according SAP business object:

* Delivery: /AEB/CL\_PA\_PB\_DLV\_SHP\_COLL\_BC
* Shipment: /AEB/CL\_PA\_PB\_SHP\_SHP\_COLL\_BC
* TM document: /AE1/CL\_PA\_PB\_FRO\_SHP\_COLL\_BC

The collector provides the data based on the standard logic and configuration of the AEB add-on, plus the additional logic from the BAdIs that are implemented. Background: To change or add something for the collected data, you can implement different BAdIs for the according business object:  [BAdIs to change data ](https://sap-plugins.docs.developers.aeb.com/docs/badis-to-change-the-exported-data-1).  

Here is an example of how to collect the data for an outbound delivery:  

```text Collect the data
REPORT zaeb_create_first_shipment.

PARAMETERS: vbeln TYPE likp-vbeln.

DATA:
  likp             TYPE likp,
  lipss            TYPE STANDARD TABLE OF lips,
  vbpas            TYPE STANDARD TABLE OF vbpa,
  vekps            TYPE STANDARD TABLE OF vekp,
	vepos            TYPE STANDARD TABLE OF vepo,
  org_unit_rule_bc TYPE REF TO /aeb/cl_pa_pb_dlv_ou_rule_bc,
  org_unit         TYPE /aeb/01_char20,
  engn_prm_bc      TYPE REF TO /aeb/cl_pa_pb_engn_prm_bc,
  engn_prm_mo      TYPE REF TO /aeb/if_pa_pb_engn_prm_mo,
  collector_bc     TYPE REF TO /aeb/cl_pa_pb_dlv_shp_coll_bc,
  shipment         TYPE /aeb/pa_pb_dl_shp_req_do.

SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.
SELECT * FROM lips INTO TABLE lipss WHERE vbeln = vbeln.
SELECT * FROM vbpa INTO TABLE vbpas WHERE vbeln = vbeln.
"Determine vekps for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB
"Determine vepos for example via HU_PER_DOCUMENT_READ or HU_GET_HUS_DB

org_unit_rule_bc = /aeb/cl_pa_pb_dlv_ou_rule_bc=>new_for( im_likp = likp im_lipss = lipss ).
org_unit = org_unit_rule_bc->get_org_unit( ).


collector_bc = /aeb/cl_pa_pb_dlv_shp_coll_bc=>new_for( im_likp = likp
                                                       im_lipss = lipss
                                                       im_vbpas = vbpas
                                                       im_vekps = vekps
																											 im_vepos = vepos ).

shipment = collector_bc->create_shipment( org_unit ).

write shipment-referencenumber1.
WRITE /.
```

 With this example you have collected the shipping data of the SAP delivery, e.g. the ship-to address and the carrier. 

To change or add something for the collected data, you can implement the "AFTER\_STD\_FILLING"-method of the BAdI for the according business object:  [BAdIs to change data ](https://sap-plugins.docs.developers.aeb.com/docs/badis-to-change-the-exported-data-1). 

## Determine the workstation

The workstation ID is mandatory for creating a shipping order and printing labels.  Just add the following code lines to the program: 

```text Set Workstation ID
DATA: workstation_id TYPE string.
workstation_id = /aeb/cl_pa_pb_wsta_id_bc=>new( )->get_workstation_id( ).
write: workstation_id.
write /.
```

## Determine the reference number

In certain scenarios & calls the reference number of the shipping order is required.  During the creation of a shipping order in Carrier Connect a unique reference is assigned. By using this function, you can retrieve that reference ID for the according SAP business object:

```text
REPORT zaeb_create_first_shipment.

PARAMETERS: vbeln TYPE likp-vbeln.

DATA:
likp						 TYPE likp,
lo_dlv_shp_ref   TYPE REF TO /aeb/cl_pa_pb_dlv_shp_ref_bc,
ls_shp_ref        TYPE /aeb/pa_pb_dl_shp_ref_do.

SELECT SINGLE * FROM likp INTO likp WHERE vbeln = vbeln.
lo_dlv_shp_ref = /aeb/cl_pa_pb_dlv_shp_ref_bc=>new_for( im_likp = likp ).
ls_shp_ref = lo_dlv_shp_ref->get_shp_ref_do( ).
write: ls_shp_ref.
write /.
```

## Further subsequent functionality

Using the API class you can make use of further functionality:   

[Create shipping order (Create shipment)](https://sap-plugins.docs.developers.aeb.com/docs/create-shipment)

[Upddate shipping order (Process Shipment)](https://sap-plugins.docs.developers.aeb.com/docs/update-shipping-order-process-shipment)

[Read shipping order data (Get Shipments)](https://sap-plugins.docs.developers.aeb.com/docs/read-data-get-shipments)

[Attach documents](https://sap-plugins.docs.developers.aeb.com/docs/attach-documents-to-a-shipping-order)

[Export process and paperless trade](https://sap-plugins.docs.developers.aeb.com/docs/export-process-paperless-trade)

[Pickup processing (Create and process pickup)](https://sap-plugins.docs.developers.aeb.com/docs/pickup-processing-create-and-update-pickup)

[GetShipment for price determination](https://dash.readme.com/project/sap-plugin/v1.0/docs/getshipment-for-price-determination)

[GetQuotes for price/runtime determination of one or alle services of one carrier](https://dash.readme.com/project/sap-plugin/v1.0/docs/getquotes-for-priceruntime-determination-of-one-or-alle-services-of-one-carrier)
