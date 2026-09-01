---
parser: v2
auto_validation: true
primary_tag: software-product>sap-btp-abap-environment
tags: [  tutorial>beginner, programming-tool>abap-development, software-product>sap-btp-abap-environment]
time: 30
author_name: Sheena M K
author_profile: https://github.com/sheenamk
---

# Using Virtual Elements in CDS Projection Views
<!-- description --> Create and expose virtual elements in CDS Projection Views

## Prerequisites
- You have done one of the following:
    - **Tutorial**: [Create an SAP BTP ABAP Environment Trial User](abap-environment-trial-onboarding)
    - You have bought a licensed version of SAP BTP ABAP Environment
- You have installed [ABAP Development Tools](https://tools.hana.ondemand.com/#abap), latest version

## You will learn
  - How to create virtual elements in CDS projection view for fields which are not part of the data model
  - Calculate the values of virtual elements using ABAP resources during runtime

## Intro
For some business use cases, it may be necessary to add new elements to the data model of an application whose values cannot be fetched from the database. Virtual elements are used if these elements are not provided as part of the original persistence data model but can be calculated using ABAP resources during runtime. For more details refer to  [Using Virtual Elements in CDS Projection Views](https://help.sap.com/docs/abap-cloud/abap-rap/using-virtual-elements-in-cds-projection-views?version=sap_btp&locale=en-US&ai=true)


---

### Create package

1. Create a new package for this tutorial, by right clicking on **Favourtite Packages** from **Project Explorer**. Now choose **New > ABAP Package**.

2. In the wizard enter the following then follow the wizard, choosing a **new** transport request:
    - Name: **`ZABAP_VIRT_ELEM_###`**
    - Description: **Package for Virtual Elements**

     ![New Package](images/newpackage.png)

### Create database table

1. Choose your package, then choose **New > Other Repository Object** from the context menu.

2. Search for **Database Table**, select it and then click **Next>**.

     ![Database Table](images/databasetable.png)

3. Enter the following information (### is your group ID), then choose **Next**.
    - Name: **`ztravel_###`**
    - Description: **Table for Travel Data**

     ![Create Table](images/createtable.png)

4. Choose the transport request, then choose **Finish** to create the database table.

5. The table appears in a new editor.

6. Replace the default code with the code snippet below and replace all occurences of the placeholder ### with your group ID using the Replace All function (Ctrl+F).:

   ```ABAP
   @EndUserText.label : 'Database Table for ZTRAVEL###'
   @AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
   @AbapCatalog.tableCategory : #TRANSPARENT
   @AbapCatalog.deliveryClass : #A
   @AbapCatalog.dataMaintenance : #RESTRICTED
   define table ztravel_### {
    key client            : abap.clnt not null;
    key uuid              : sysuuid_x16 not null;
    key travel_id         : /dmo/travel_id not null;
    customer_id           : /dmo/customer_id;
    description           : /dmo/description;
    status                : /dmo/travel_status;
    begin_date            : abap.datn;
    end_date              : abap.datn;
    @Semantics.amount.currencyCode : 'ztravel_###.currency_code'
    flight_price          : /dmo/flight_price;
    currency_code         : /dmo/currency_code;
    local_created_by      : abp_creation_user;
    local_created_at      : abp_creation_tstmpl;
    local_last_changed_by : abp_locinst_lastchange_user;
    local_last_changed_at : abp_locinst_lastchange_tstmpl;
    last_changed_at       : abp_lastchange_tstmpl;
   }

   ```

7. Format, save, and activate your table ( **`Shift+F1, Ctrl+S, Ctrl+F3`** ).

### Generate the transactional UI service

Create your OData v4 based UI services and the RAP business object (BO) with all the needed ABAP artefacts - e.g. CDS view entities, behavior definition and implementation - using the built-in ADT generator.
The generated business service will be transactional, draft-enabled, and enriched with UI semantics for the generation of the Fiori elements app.

1. Right-click your database table table **ZTRAVEL_###** and select **Generate ABAP Repository Objects** from the context menu.
2. Select OData UI Service and click Next >.
     ![OData Service](images/odataservice.png)

3. Click Next >, enter your package name and click Next < again. 

4. Maintain the required information on the Configure Generator dialog to provide the suffix, name of your data model as shown in the below screenshot and generate them.
For that, navigate through the wizard tree (Business Objects, Data Model, etc...), maintain the artefact names and press Next >.
5. Verify the maintained entries and press Next > and Finish to confirm. The needed artefacts will be generated.
     ![Development Objects](images/developmentobjects.png)
6. Go to the Project Explorer, select your package package **`ZABAP_VIRT_ELEM_###`**, refresh it by pressing F5, and check all generated ABAP repository objects.

7. Open your service binding ZUI_TRAVEL_###_O4 and click **Publish**.
8. Double-click on the entity **Travel** in the Entity Set and Association section to open the Fiori elements App Preview.
9. Press Go to load the data in the app. The list should be empty at this point since no demo data has been created yet.


### Enhance the Data Model of the Base and Projected Business Object (BO)
Define and expose new associations in the base BO data model defined in the CDS view entity
    Associations to the business entities Customer (_Customer)
    Associations to helpful information about Status (_Status) and Currency (_Currency)

1. Define the new associations _Customer, _Status, and _Currency.
   Open your data definition data definition ZR_TRAVEL### and format the source code with the ABAP Formatter (aka Pretty Printer) by pressing Shift+F1.
   Insert the following code snippet after the select statement as shown on the screenshot below and format the source code (Shift+F1).

  ```CDS
   association [0..1] to /DMO/I_Customer as _Customer on $projection.CustomerID = _Customer.CustomerID
   association [0..1] to I_Currency as _Currency on $projection.CurrencyCode = _Currency.Currency
   association [1..1] to /DMO/I_Overall_Status_VH as _OverallStatus on $projection.Status = _OverallStatus.OverallStatus

  ```
2. Expose the defined associations _Customer, _OverallStatus and _Currency in the selection list.
   For that, insert the code snippet provided below in the selection list between the curly brackets ({...}) as shown on the screenshot and format the source code (Shift+F1).

  ```CDS
   _Customer,
   _Currency,
   _OverallStatus

   ```
   Your source code should look like this:

  ```CDS
   @AccessControl.authorizationCheck: #MANDATORY
   @Metadata.allowExtensions: true
   @ObjectModel.sapObjectNodeType.name: 'ZTRAVEL_###'
   @EndUserText.label: '###GENERATED Core Data Service Entity'
   define root view entity ZR_TRAVEL_###
    as select from ZTRAVEL_### as Travel
    association [0..1] to /DMO/I_Customer as _Customer on $projection.CustomerID = _Customer.CustomerID
    association [0..1] to I_Currency as _Currency on $projection.CurrencyCode = _Currency.Currency
    association [1..1] to /DMO/I_Overall_Status_VH as _OverallStatus on $projection.Status = _OverallStatus.OverallStatus
    {
      key uuid as UUID,
      key travel_id as TravelID,
      customer_id as CustomerID,
      description as Description,
      status as Status,
      begin_date as BeginDate,
      end_date as EndDate,
      @Semantics.amount.currencyCode: 'CurrencyCode'
        flight_price as FlightPrice,
        @Consumption.valueHelpDefinition: [ {
         entity.name: 'I_CurrencyStdVH', 
         entity.element: 'Currency', 
         useForValidation: true
     }    ]
        currency_code as CurrencyCode,
        @Semantics.user.createdBy: true
        local_created_by as LocalCreatedBy,
        @Semantics.systemDateTime.createdAt: true
        local_created_at as LocalCreatedAt,
        @Semantics.user.localInstanceLastChangedBy: true
        local_last_changed_by as LocalLastChangedBy,
        @Semantics.systemDateTime.localInstanceLastChangedAt: true
        local_last_changed_at as LocalLastChangedAt,
        @Semantics.systemDateTime.lastChangedAt: true
        last_changed_at as LastChangedAt,
        _Customer,
        _Currency,
        _OverallStatus
     }

   ```

3. Save and activate activate the changes.

4. Enhance the CDS data model of the projected travel BO entity with new elements from associations, adjusting the defined value help definitions and removing use case irrelevant elements.
Replace the whole data definition of the travel BO projection view datadefinition ZC_Travel### with the source code from the document provided below.

  ```CDS
    @Metadata.allowExtensions: true
    @Metadata.ignorePropagatedAnnotations: true
    @EndUserText: {
        label: '###GENERATED Core Data Service Entity'
        }
    @ObjectModel: {
    sapObjectNodeType.name: 'ZTRAVEL_###'
    }
    @AccessControl.authorizationCheck: #MANDATORY
    define root view entity ZC_TRAVEL_###
    provider contract transactional_query
    as projection on ZR_TRAVEL_###
    association [1..1] to ZR_TRAVEL_### as _BaseEntity on $projection.UUID = _BaseEntity.UUID and $projection.  TravelID = _BaseEntity.TravelID
    {
        key UUID,
        key TravelID,
        @Consumption.valueHelpDefinition: [{entity: {name: '/DMO/I_Customer_StdVH', element: 'CustomerID' }, useForValidation: true}]
        @ObjectModel.text.element: ['CustomerName']
        @Search.defaultSearchElement: true
        CustomerID as CustomerID,
        _Customer.FirstName as CustomerName,  
        Description,
        @ObjectModel.text.element: ['OverallStatusText'] //case-sensitive
        @Consumption.valueHelpDefinition: [{ entity: {name: '/DMO/I_Overall_Status_VH', element: 'OverallStatus' }, useForValidation: true }]
        Status,
        _OverallStatus._Text.Text as OverallStatusText : localized,
        BeginDate,
        EndDate,
        @Semantics: {
            amount.currencyCode: 'CurrencyCode'
            }
        FlightPrice,
        @Consumption: {
            valueHelpDefinition: [ {
            entity.element: 'Currency', 
            entity.name: 'I_CurrencyStdVH', 
            useForValidation: true
            }       ]
         }
        CurrencyCode,
        @Semantics: {
         user.createdBy: true
         }
        LocalCreatedBy,
        @Semantics: {
            systemDateTime.createdAt: true
         }
        LocalCreatedAt,
        @Semantics: {
         user.localInstanceLastChangedBy: true
            }
        LocalLastChangedBy,
        @Semantics: {
            systemDateTime.localInstanceLastChangedAt: true
            }
        LocalLastChangedAt,
        @Semantics: {
            systemDateTime.lastChangedAt: true
            }
        LastChangedAt,
        _BaseEntity
    }

   ```
5. Save and activate activate the changes.

6. Preview and Test the Enhanced Travel App
    You can now preview and test the changes by creating a new travel instance in the Travel app.
    1. Refresh your application in the browser using F5 if the browser is still open -
       or go to your service binding ZUI_TRAVEL_###_O4 and start the Fiori elements App preview for the Travel entity set.
    2. Create a new Travel instance.
       A dialog for manually entering a Travel ID should be displayed now. When you create a new entry you see that the value helps for the fields Customer ID offer an out of the box frontend validation. Enter some demo data required as shown below:
         ![Demo Data](images/demodata.png)

### Define the virtual elements
Now, go ahead and define 2 virtual elements **Trip Duration** and **Remaining Days to Flight** that will be used to specify the duration of the trip and number of days left before the flight date.

1. Create an ABAP class ZCL_CALC_VIRT_ELEM_###, save and activate it. This class will be used to handle the logic to populate the virtual element values. 
    ![ABAP Class](images/abapclassnew.png)

2. Now add the statements to declare the virtual elements in the data definition of the travel BO projection view ZC_TRAVEL_### after the OverallStatusText in the SELECT list.
The keyword virtual must be specified in front of the element and the name of the calculation class must be specified in the annotation @ObjectModel.virtualElementCalculatedBy. The ABAP class ZCL_CALC_VIRT_ELEM_### created above will be used to calculate this virtual element is specified.

        
      ```CDS
        @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CALC_VIRT_ELEM_###'
        @EndUserText.label: 'Trip Duration'
      virtual TripDuration : abap.int2,

        @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CALC_VIRT_ELEM_###'
        @EndUserText.label: 'Remaining Days to Flight'
      virtual RemainingDaysToFlight  : abap.int1,
    
      ``` 
     
   Your code should look like this:

      ```CDS
     @Metadata.allowExtensions: true
     @Metadata.ignorePropagatedAnnotations: true
     @EndUserText: {
        label: '###GENERATED Core Data Service Entity'
        }
     @ObjectModel: {
     sapObjectNodeType.name: 'ZTRAVEL_###'
     } 
     @AccessControl.authorizationCheck: #MANDATORY
     define root view entity ZC_TRAVEL_###
     provider contract transactional_query
     as projection on ZR_TRAVEL_###
     association [1..1] to ZR_TRAVEL_### as _BaseEntity on  $projection.UUID     = _BaseEntity.UUID
                                                    and $projection.TravelID = _BaseEntity.TravelID
     {
        key     UUID,
        key     TravelID,
          @Consumption.valueHelpDefinition: [{entity: {name: '/DMO/I_Customer_StdVH', element: 'CustomerID' }, useForValidation: true}]
          @ObjectModel.text.element: ['CustomerName']
          @Search.defaultSearchElement: true
          CustomerID                as CustomerID,
          _Customer.FirstName       as CustomerName,
          Description,
          @ObjectModel.text.element: ['OverallStatusText'] //case-sensitive
          @Consumption.valueHelpDefinition: [{ entity: {name: '/DMO/I_Overall_Status_VH', element: 'OverallStatus' }, useForValidation: true }]
          Status,
          _OverallStatus._Text.Text as OverallStatusText : localized,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CALC_VIRT_ELEM_###'
          @EndUserText.label: 'Trip Duration'
     virtual TripDuration          : abap.int2,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CALC_VIRT_ELEM_###'
          @EndUserText.label: 'Remaining Days to Flight'
     virtual RemainingDaysToFlight : abap.int1,
          BeginDate,
          EndDate,
          @Semantics: {
              amount.currencyCode: 'CurrencyCode'
              }
          FlightPrice,
          @Consumption: {
              valueHelpDefinition: [ {
              entity.element: 'Currency',
              entity.name: 'I_CurrencyStdVH',
              useForValidation: true
              }       ]
           }
          CurrencyCode,
          @Semantics: {
           user.createdBy: true
           }
          LocalCreatedBy,
          @Semantics: {
              systemDateTime.createdAt: true
           }
          LocalCreatedAt,
          @Semantics: {
           user.localInstanceLastChangedBy: true
              }
          LocalLastChangedBy,
          @Semantics: {
              systemDateTime.localInstanceLastChangedAt: true
              }
          LocalLastChangedAt,
          @Semantics: {
              systemDateTime.lastChangedAt: true
              }
          LastChangedAt,
          _BaseEntity
     }
    
      ``` 

### Calculate the Virtual Elements of the Travel BO Entity
Implement the logic of the virtual elements **Trip Duration** and **Remaining Days to Flight** in the ABAP Class ZCL_CALC_VIRT_ELEM_###.

1. Open your ABAP class ZCL_CALC_VIRT_ELEM_### and replace the entire code with the code provided below. Replace all occurences of the placeholder ### with your assigned suffix using Ctrl+F.

      ```ABAP
     CLASS zcl_calc_virt_elem_### DEFINITION
       PUBLIC
        FINAL
       CREATE PUBLIC .

       PUBLIC SECTION.
        INTERFACES if_sadl_exit_calc_element_read .
       PROTECTED SECTION.
       PRIVATE SECTION.
      ENDCLASS.


      CLASS zcl_calc_virt_elem_### IMPLEMENTATION.
       METHOD if_sadl_exit_calc_element_read~calculate.
        IF it_requested_calc_elements IS INITIAL.
         EXIT.
        ENDIF.

         LOOP AT it_requested_calc_elements ASSIGNING FIELD-SYMBOL(<fs_req_calc_elements>).
         CASE <fs_req_calc_elements>.
          "virtual elements from BOOKING entity
         WHEN 'TRIPDURATION'   OR 'REMAININGDAYSTOFLIGHT'.
          DATA lt_book_original_data TYPE STANDARD TABLE OF zc_travel_### WITH DEFAULT KEY.
          lt_book_original_data = CORRESPONDING #( it_original_data ).
          LOOP AT lt_book_original_data ASSIGNING FIELD-SYMBOL(<fs_book_original_data>).

         *<fs_book_original_data> = zcl_calc_virt_elem_###=>calculate_days_to_flight(   <fs_book_original_data> ).

          ENDLOOP.
          ct_calculated_data = CORRESPONDING #( lt_book_original_data ).
         ENDCASE.
         ENDLOOP.
        ENDMETHOD.

         METHOD if_sadl_exit_calc_element_read~get_calculation_info.
         IF iv_entity EQ 'ZR_TRAVEL_###'. "Booking BO node
         LOOP AT it_requested_calc_elements ASSIGNING FIELD-SYMBOL(<fs_booking_calc_element>).
          CASE <fs_booking_calc_element>.
          WHEN 'TRIPDURATION'.
            COLLECT `BOOKINGDATE` INTO et_requested_orig_elements.
            COLLECT `FLIGHTDATE` INTO et_requested_orig_elements.
          WHEN 'REMAININGDAYSTOFLIGHT'.
            COLLECT `FLIGHTDATE` INTO et_requested_orig_elements.
          ENDCASE.
          ENDLOOP.
         ENDIF.
         ENDMETHOD.
       ENDCLASS.
    
      ``` 

The class implements the virtual element interface IF_SADL_EXIT_CALC_ELEMENT_READ that must be implemented by calculation classes for virtual elements.

The method IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO provides a list of all elements that are required for calculating the values of the virtual elements in the requested entity. This method is called during runtime before the retrieval of data from the database to ensure that all necessary elements for calculation are filled with data.

The method IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE executes the value calculation for the virtual element. This method is called during runtime after data is retrieved from the database. The elements needed for the calculation of the virtual elements are already inside the data table passed to this method. The method returns a table that contains the values of the requested virtual elements.

2. Define the class method calculate_days_to_flight in the public section of the class definition where the proper calculation of the virtual elements TripDuration and RemainingDaysToFlight will take place. The method is declared as class method to have the possibility to access it externaly, for example, from a function.

For that, insert the code snippet provided below after the statement interfaces IF_SADL_EXIT_CALC_ELEMENT_READ. in the class definition and replace all occurences of the placeholder ### with your assigned suffix.

      ```ABAP
      CLASS-METHODS:
       calculate_days_to_flight
        IMPORTING is_original_data TYPE ZC_TRAVEL_###
        RETURNING VALUE(result)    TYPE ZC_TRAVEL_###.
    
      ``` 

3. Press the light bulb symbol on the left side or use the ADT Quick Fix (Ctrl+1) to add the missing method implementations. Set the cursor before your method calculate_trav_status_ind and press CTRL + 1, select Add implementation for calculate_trav_status_ind.

     ![Calculation Method](images/addcalcmethod.png)

4. Implement the method calculate_trav_status_ind which calculates the value of the virtual element defined in the Travel BO entity.
For that, replace the empty method implementation of calculate_days_to_flight with the code snippet provided below.

      ```ABAP
      METHOD calculate_days_to_flight.
        DATA(today) = cl_abap_context_info=>get_system_date( ).
        result = CORRESPONDING #( is_original_data ).

        "VE Trip Duration: Number of between start date and end date of trip
        DATA(trip_duration) = result-EndDate - result-BeginDate.
        IF trip_duration > 0 and trip_duration < 999.
          result-TripDuration =  trip_duration.
        ELSE.
            result-TripDuration = 0.
        ENDIF.

        "VE RemainingDaysToFlight: remaining days to flight date from today
        DATA(remaining_days) = result-BeginDate - today.
        IF remaining_days < 0 OR remaining_days > 999.
            result-RemainingDaysToFlight = 0.
        ELSE.
         result-RemainingDaysToFlight =  result-BeginDate - today.
        ENDIF.
      ENDMETHOD.

      ``` 

TripDuration : The number of days between the flight begin date and the flight end date (flight end date - flight begin date).
RemainingDaysToFlight: The number of days until departure from today.

5. Now, uncomment the method call calculate_trav_status_ind within the method CALCULATE

      ```ABAP
      <fs_book_original_data> = zcl_calc_virt_elem_222=>calculate_days_to_flight( <fs_book_original_data> ).
      ``` 

     ![Uncomment Command](images/uncomment.png)

6. Save and activate (Ctrl+F3) the changes. Close the ABAP class.
    
### Preview and Test the enhanced Travel App
Test the enhanced SAP Fiori elements application.
1. Refresh your browser or start the SAP Fiori elements app preview from your service binding ../ servicebinding ZUI_TRAVEL_123_O4 by double-clicking the Travel entity set.

2. Click Go on the app and check the result.

3. Press the respective Gear icon and add the missing column (virtual elements) on the list report and click OK.
 ![Add Fields](images/addfields.png)

4. Now you can see the 2 virtual element fields 'Trip Duration' and 'Remaining Days To Flight'.
 ![Virtual Elements](images/virtualelements.png)

### Test yourself

### More information
- SAP Help Portal: [Generating a RAP Business Service with the Generate ABAP Repository Objects Wizard](https://help.sap.com/viewer/923180ddb98240829d935862025004d6/Cloud/en-US/945d84d4981b427ab5ea9129d344c8d8.html)
- SAP Help Portal: [Modeling Virtual Elements](https://help.sap.com/docs/abap-cloud/abap-rap/modeling-virtual-elements?locale=en-US&ai=true)

---