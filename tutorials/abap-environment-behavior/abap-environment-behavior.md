---
parser: v2
auto_validation: true
primary_tag: software-product>sap-btp--abap-environment
tags: [  tutorial>beginner, programming-tool>abap-development, software-product>sap-business-technology-platform ]
time: 15
author_name: Merve Temel
author_profile: https://github.com/mervey45
---

# Create Behavior Definition for Managed Scenario
<!-- description --> Create behavior definition and implementation for managed scenario.

## Prerequisites  
- You need to have access to an SAP BTP, ABAP environment, or SAP S/4HANA Cloud, ABAP environment or SAP S/4HANA (release 2021 or higher) system.
For example, you can create [free trial user on SAP BTP, ABAP environment](abap-environment-trial-onboarding).
- You have downloaded and installed the [latest ABAP Development Tools (ADT)] (https://tools.hana.ondemand.com/#abap) on the latest Eclipse© platform.
- If you are using a **licensed system** make sure, that the flight scenario is available in your system. If not, you should follow these [steps](https://help.sap.com/docs/btp/sap-abap-restful-application-programming-model/downloading-abap-flight-reference-scenario) in your licensed system. The flight scenario is already in the trial systems.

## You will learn  
  - How to create behavior definition
  - How to create behavior implementation
  - How to create behavior definition for projection view

## Intro
In this tutorial, wherever ### appears, use a number (e.g. 000).

---

### Create behavior definition

  1. Right-click on your data definition `ZR_TRAVELTP_###` and select **New Behavior Definition**. 

      ![Create behavior definition](definition.png)

  2. Check your behavior definition. Your **implementation type** is **managed**.

     Click **Next >**.

      ![Create behavior definition](definition2.png)

  3. Click **Finish** to use your transport request.

      ![Create behavior definition](definition3.png)

  4. Replace your code with following.

    ```ABAP
    managed implementation in class zbp_r_traveltp_### unique;
    strict ( 2 );

    define behavior for ZR_TravelTP_### alias Travel
    persistent table zatravel_###
    authorization master ( instance )
    etag master lastchangedat
    lock master
    {

      // semantic key is calculated in a determination
      field ( readonly ) travelid;

      // administrative fields (read only)
      field ( readonly ) lastchangedat, lastchangedby, createdat, createdby;

      // mandatory fields that are required to create a travel
      field ( mandatory ) agencyid, overallstatus, bookingfee, currencycode;

      // mandatory fields that are required to create a travel
      field ( mandatory ) BeginDate, EndDate, CustomerID;

      // standard operations for travel entity
      create;
      update;
      delete;
    }
    ```
 

  5. Save and activate.

      ![save and activate](activate.png)

    A warning will appear first, but after the creation of the behavior implementation it will disappear.  

    Now the **behavior definition** is created and determines the create, update and delete functionality for travel booking.

 
   
### Create behavior definition for projection view

  1. Right-click on your data definition `ZC_TRAVELTP_###` and select **New Behavior Definition**.

      ![Create behavior definition for projection view](projection.png)

  2. Check your behavior definition. Your implementation type is projection.

     Click **Next >**.
 
      ![Create behavior definition for projection view](projection2.png)

  3. Click **Finish** to use your transport request.

      ![Create behavior definition for projection view](projection3.png)

  4. Add your alias to the behavior definition for the projection view. Replace your code with following:

    ```ABAP
    projection;
    strict ( 1 ); 

    define behavior for ZC_TravelTP_### alias TravelProcessor
    {
      use create;
      use update;
      use delete;
    }
    ```

  5. Save and activate.

      ![save and activate](activate.png)



### Test yourself



---
