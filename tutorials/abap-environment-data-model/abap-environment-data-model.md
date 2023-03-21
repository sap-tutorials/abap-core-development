---
parser: v2
auto_validation: true
primary_tag: software-product>sap-btp--abap-environment
tags: [  tutorial>beginner, programming-tool>abap-development, software-product>sap-business-technology-platform ]
time: 20
author_name: Merve Temel
author_profile: https://github.com/mervey45
---

# Define and Expose a CDS-Based Travel Data Model
<!-- description --> Define CDS-based data model and create projection view.

## Prerequisites  
- You need to have access to an SAP BTP, ABAP environment, or SAP S/4HANA Cloud, ABAP environment or SAP S/4HANA (release 2021 or higher) system.
For example, you can create [free trial user on SAP BTP, ABAP environment](abap-environment-trial-onboarding).
- You have downloaded and installed the [latest ABAP Development Tools (ADT)] (https://tools.hana.ondemand.com/#abap) on the latest Eclipse© platform.
- If you are using a **licensed system** make sure, that the flight scenario is available in your system. If not, you should follow these [steps](https://help.sap.com/docs/btp/sap-abap-restful-application-programming-model/downloading-abap-flight-reference-scenario) in your licensed system. The flight scenario is already in the trial systems.

## You will learn   
  - How to create CDS based data model
  - How to create projection view
  - How to create service definition
  - How to create service binding  

## Intro
In this tutorial, wherever ### appears, use a number (e.g. 000).

---

>**If you also want to deploy your SAP Fiori application, please finish this tutorial first and then continue with** [ Develop a Fiori App Using the ABAP RESTful Application Programming Model (Managed Scenario)](group.abap-env-restful-managed) **starting with following tutorial** [Create Behavior Definition for Managed Scenario](abap-environment-behavior).
 
### Define CDS-based travel data model

  1. Right-click on database table `ZATRAVEL_###` and select **New Data Definition**.

      ![Define CDS based travel data model](model.png)

  2. Create a data definition:

     - Name: `ZR_TravelTP_###`
     - Description: `Data model for travel`

     Click **Next >**.

      ![Define CDS based travel data model](model2.png)

  3. Click **Finish** to use your transport request.

      ![Define CDS based travel data model](model3.png)
 

  4. Add associations, fields, UI semantics and the `root` node to your data definition. Therefore replace your code with following:

    ```ABAP
    @EndUserText.label: 'Travel data ###'
    @AccessControl.authorizationCheck: #CHECK
    define root view entity ZR_TravelTP_### as select from zatravel_### 

      /* Associations */
      association [0..1] to /DMO/I_Agency   as _Agency   on $projection.AgencyID = _Agency.AgencyID
      association [0..1] to /DMO/I_Customer as _Customer on $projection.CustomerID = _Customer.CustomerID
      association [0..1] to I_Currency      as _Currency on $projection.CurrencyCode = _Currency.Currency

    {
      key mykey                 as Mykey,
          travel_id             as TravelID,
          agency_id             as AgencyID, 
          customer_id           as CustomerID,
          begin_date            as BeginDate,
          end_date              as EndDate,
          @Semantics.amount.currencyCode: 'CurrencyCode'
          booking_fee           as BookingFee,
          @Semantics.amount.currencyCode: 'CurrencyCode'
          total_price           as TotalPrice,
          currency_code         as CurrencyCode,
          description           as Description,
          overall_status        as OverallStatus,
          created_by            as CreatedBy,
          created_at            as CreatedAt,
          last_changed_by       as LastChangedBy,
          @Semantics.systemDateTime.localInstanceLastChangedAt: true
          last_changed_at       as LastChangedAt,
          @Semantics.systemDateTime.lastChangedAt: true
          local_last_changed_at as LocalLastChangedAt,

          /* Public associations */
          _Agency,
          _Customer,
          _Currency
    } 
    ```

  5. Save and activate.

      ![save and activate](activate.png)

     Your CDS view for travel booking is defined now. You can use and manipulate data that is persisted in your database.

 
### Create projection view for travel

  1. Right-click on data definition `ZR_TRAVELTP_###` and select **New Data Definition**.

      ![Create projection view for travel](projection.png)

  2. Create a data definition:

     - Name: `ZC_TRAVELTP_###`
     - Description: `Projection view for travel`

     Click **Next >**.

      ![Create projection view for travel](projection2.png)

  3. Click **Finish** to use your transport request.

      ![Create projection view for travel](projection3.png)

  4. Replace your code with following:

    ```ABAP
    @AccessControl.authorizationCheck: #NOT_REQUIRED
    @EndUserText.label: 'Projection view for travel'

    @UI: {
    headerInfo: { typeName: 'Travel', typeNamePlural: 'Travels', title: { type: #STANDARD, value: 'TravelID' } } }

    @Search.searchable: true

    define root view entity ZC_TravelTP_###
      as projection on ZR_TravelTP_###
    {
          @UI.facet: [ { id:              'Travel',
                        purpose:         #STANDARD,
                        type:            #IDENTIFICATION_REFERENCE,
                        label:           'Travel',
                        position:        10 } ]

          @UI.hidden: true
      key Mykey              as TravelUUID,


          @UI: {
              lineItem:       [ { position: 10, importance: #HIGH } ],
              identification: [ { position: 10, label: 'Travel ID [1,...,99999999]' } ] }
          @Search.defaultSearchElement: true
          TravelID          as TravelID,

          @UI: {
              lineItem:       [ { position: 20, importance: #HIGH } ],
              identification: [ { position: 20 } ],
              selectionField: [ { position: 20 } ] }
          @Consumption.valueHelpDefinition: [{ entity : {name: '/DMO/I_Agency', element: 'AgencyID'  } }]

          @ObjectModel.text.element: ['AgencyName'] ----meaning?
          @Search.defaultSearchElement: true
          AgencyID          as AgencyID,
          _Agency.Name       as AgencyName,

          @UI: {
              lineItem:       [ { position: 30, importance: #HIGH } ],
              identification: [ { position: 30 } ],
              selectionField: [ { position: 30 } ] }
          @Consumption.valueHelpDefinition: [{ entity : {name: '/DMO/I_Customer', element: 'CustomerID'  } }]

          @ObjectModel.text.element: ['CustomerName']
          @Search.defaultSearchElement: true
          CustomerID        as CustomerID,

          @UI.hidden: true
          _Customer.LastName as CustomerName,

          @UI: {
              lineItem:       [ { position: 40, importance: #MEDIUM } ],
              identification: [ { position: 40 } ] }
          BeginDate         as BeginDate,

          @UI: {
              lineItem:       [ { position: 41, importance: #MEDIUM } ],
              identification: [ { position: 41 } ] }
          EndDate           as EndDate,

          @UI: {
              lineItem:       [ { position: 50, importance: #MEDIUM } ],
              identification: [ { position: 50, label: 'Total Price' } ] }
          @Semantics.amount.currencyCode: 'CurrencyCode'
          TotalPrice        as TotalPrice,

          @Consumption.valueHelpDefinition: [{entity: {name: 'I_Currency', element: 'Currency' }}]
          CurrencyCode      as CurrencyCode,

          @UI: {
          lineItem:       [ { position: 60, importance: #HIGH },
                            { type: #FOR_ACTION, dataAction: 'acceptTravel', label: 'Accept Travel' } ],
          identification: [ { position: 60, label: 'Status [O(Open)|A(Accepted)|X(Canceled)]' } ]  }
          OverallStatus     as TravelStatus,

          @UI.identification: [ { position: 70, label: 'Remarks' } ]
          Description        as Description,

          @UI.hidden: true
          LastChangedAt    as LastChangedAt

    }
    ```

  5. Save and activate.

      ![save and activate](activate.png)

     The **projection** is created and includes UI annotations. The projection is the subset of the fields of the travel data model, which are relevant for the travel booking application.



### Create service definition

  1. Right-click on projection view `ZC_TRAVELTP_###` and select **New Service Definition**.

      ![Create service definition](definition.png)

  2. Create a new service definition:

     - Name: `ZUI_TRAVEL_###`
     - Description: `Service definition for travel`

     Click **Next >**.

      ![Create service definition](definition2.png)

  3. Click **Finish** to use your transport request.

      ![Create service definition](definition3.png)

  4. Expose these entities:

    ```ABAP
    @EndUserText.label: 'Service definition for travel'
    define service ZUI_Travel_### {
      expose ZC_TravelTP_### as TravelProcessor;
      expose /DMO/I_Customer as Passenger;
      expose /DMO/I_Agency as TravelAgency;
      expose /DMO/I_Airport as Airport;
      expose I_Currency as Currency;
      expose I_Country as Country;
    }  
    ```

  5. Save and activate.

      ![save and activate](activate.png)

     With the **service definition** you are able to define which data is exposed as a business service in your travel booking application.


### Create service binding

  1. Right-click on service definition `ZUI_TRAVEL_###` and select **New Service Binding**.

      ![Create service binding](binding.png)

  2. Create a new service binding:

     - Name: `ZUI_TRAVEL_O4_###` 
     - Description: `Service binding for travel`
     - Binding Type: `ODATA V4 - UI`
 
     Click **Next >**.

      ![Create service binding](binding2.png)

  3. Click **Finish** to use your transport request.

      ![Create service binding](binding3.png)

  4. **Activate** your service binding and then **publish** it.

      ![Create service binding](binding4.png)

     Now your service binding is created and you are able to see your service with its entities and associations.

      ![Create service binding](binding5.png)

     The **service binding** allows you to bind the service definition to an ODATA protocol. Therefore you are able to see the travel booking application on the UI.

### Test yourself

---