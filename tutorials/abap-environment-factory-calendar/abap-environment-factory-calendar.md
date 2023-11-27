---
auto_validation: true
time: 25
tags: [ tutorial>intermediate, programming-tool>abap-development,  softwareproducts>sap-business-technology-platform, tutorial>license ]
primary_tag: products>sap-btp--abap-environment
author_name: Achim Seubert
author_profile: https://github.com/AchimSeubert
parser: v2
---

# Use Factory Calendars in a Business Object in SAP BTP ABAP environment

<!-- description --> Create a business object that consumes factory calendars using ABAP Cloud.

## Prerequisites

- You have an SAP BTP, ABAP environment license.
- You have a business user with developer authorization (business role `SAP_BR_DEVELOPER`) in the system.
- You have downloaded and installed the [latest ABAP Development Tools (ADT)](https://tools.hana.ondemand.com/#abap) on the latest Eclipse© platform.
- You have created an ABAP Cloud project for the system in ADT.

## You will learn

- How to add a value help for factory calendars
- How to access factory calendar information from your ABAP code
- How to use factory calendars in CDS via CDS scalar functions

## Intro

The business scenario used in this tutorial is that of an online shop or ordering application. Users can create orders, providing as input a production start date, the number of days required for the order to be produced, as well as one of the available factory calendars. This information is used by the application to dynamically calculate the production end date, taking into account the working days defined by the specified factory calendar. The user can further input a delivery date, which will be validated using the same factory calendar to make sure that this date is not earlier than the calculated production end date.

Please keep in mind that this tutorial uses standard, SAP-delivered factory calendars. The creation and configuration of custom factory calendars is not part of this tutorial.

> In this tutorial, wherever ### appears, use a number (e.g., 000).

For more information on scalar functions, please see [CDS Scalar Functions | SAP Help Portal](https://help.sap.com/docs/btp/sap-abap-cds-development-user-guide/cds-scalar-functions?version=Cloud).

For more information on factory calendars, please see [Maintain Factory Calendars | SAP Help Portal](https://help.sap.com/docs/sap-btp-abap-environment/abap-environment/maintain-factory-calendars). For accessing factory calendar related information from your ABAP code, see [Factory Calendar | SAP Help Portal](https://help.sap.com/docs/btp/sap-business-technology-platform/rel-comp-factory-calendar).

---

### Create your own ABAP Package

Create your exercise package. This ABAP package will contain all the artefacts that will be created in this tutorial.

1. In ADT, go to the Project Explorer, right-click on the package **ZLOCAL**, and select **New > ABAP Package** from the context menu.

2. Maintain the required information:
    <ol type="a"><li>Name: `ZFACTORYORDER_###`
    </li><li>Description: `Factory Calendar Tutorial`
    </li><li>Select the box **Add to favourite packages**</li></ol>

    Click **Next**.

3. An application component is not needed. Click **Next**.

4. Select an existing transport request or create a new one and click **Finish**.

### Create a Database Table

Create a database table to store the underlying data of the order application.

1. Right-click on your ABAP package and select **New > Other ABAP Repository Object** from the context menu.

2. Search for **Database Table**, select it, and click **Next**.

3. Maintain the required information:
    <ol type="a"><li>Name: `ZFACTORYORD_###`
    </li><li>Description: `Factory Calendar Tutorial - Order data`
    </li></ol>

    Click **Next**.

4. Select a transport request and click **Finish**.

5. Replace the default code with the code snippet provided below (make sure to replace the placeholder ###):

    ```ABAP
    @EndUserText.label : 'Factory Calendar Tutorial - Order data'
    @AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
    @AbapCatalog.tableCategory : #TRANSPARENT
    @AbapCatalog.deliveryClass : #A
    @AbapCatalog.dataMaintenance : #RESTRICTED
    define table zfactoryord_### {

      key client            : abap.clnt not null;
      key order_id          : sysuuid_x16 not null;
      description           : abap.char(100);
      days_to_produce       : abap.int4;
      factory_calendar      : abap.char(32);
      production_start_date : abap.dats;
      delivery_date         : abap.dats;
      created_by            : abp_creation_user;
      created_at            : abp_creation_tstmpl;
      last_changed_by       : abp_lastchange_user;
      last_changed_at       : abp_lastchange_tstmpl;
      local_last_changed_at : abp_locinst_lastchange_tstmpl;

    }
    ```

6. Save and activate the changes

### Create a Business Object

Use the **Generate ABAP Repository Objects Wizard** built into ADT to create a business object on top of your database table. This will generate a full stack of objects, following the **ABAP RESTful Application Programming Model** (RAP), which results in a transactional UI service.

1. Right-click on your database table and select **Generate ABAP Repository Objects**.
2. Select option **OData UI Service** and click **Next**.
3. Choose the previously created package and click **Next**.
4. Maintain the following description and names for the objects that will be generated:

    |  Field                      | Name                      |
    |  :--------------------------| :-------------------------|
    |  Description                | `Factory Calendar Tutorial` |
    |  Data Definition Name       | `ZR_FACTORYORDER_###`     |
    |  Alias                      | `FactoryOrder`              |
    |  Implementation Class       | `ZBP_R_FACTORYORDER_###` |
    |  Draft Table Name           | `ZFACTORYORDD_###`       |
    |  Service Projection         | `ZC_FACTORYORDER_###`    |
    |  Service Definition Name    | `ZUI_FACTORYORDER_###_O4` |
    |  Service Binding Name       | `ZUI_FACTORYORDER_###_O4` |

    Click **Next**, review the objects that will be created, and click **Next** again.

5. Select a transport request and click **Finish**.
6. Publish the generated Service Binding.
7. Select the entity set **`FactoryOrder`** and launch the **Preview**. Create a new factory order to get familiar with the initial state of your application.

### Define a Custom Value Help for choosing a Factory Calendar

Currently, a user needs to know the desired factory calendar ID to be able to input it on the UI. In this step you will define a value help to improve the usability. To this end, you can use the released CDS view **`I_FactoryCalendarValueHelp`**.

1. Open the projection layer CDS view, `ZC_FACTORYORDER_###`.

2. Add the below annotation to the `FactoryCalendar` field:

    ```ABAP
    @Consumption.valueHelpDefinition: [{ entity : { name: 'I_FactoryCalendarValueHelp', element: 'FactoryCalendarID' } }]
          FactoryCalendar,
    ```
  
3. Save and activate the CDS view.

4. Reload the service binding **Preview** and check that the value help is displayed correctly.
<!-- border -->
![Preview](screenshot_step4.png)

### Define a Validation for the Factory Calendar Selection

The value help allows selecting a factory calendar from the list. However, the user can still input an arbitrary string in the corresponding field. For this reason, you will now introduce a validation that checks the existence of the specified factory calendar ID.

1. Open the interface layer behavior definition `ZR_FACTORYORDER_###`.

2. Define a new validation on the factory calendar field, which shall be triggered during the save process. Add the new validation to the standard **Prepare** action:

    ```ABAP
    validation checkFactoryCalendarId on save { field FactoryCalendar; }
    draft determine action Prepare { validation checkFactoryCalendarId; }
    ```

3. Save and activate your changes.

4. Use the quick fix that is offered to generate the required method shell in your behavior implementation class. The IDE will automatically navigate to the class.

5. Implement the method as shown below:

    ```ABAP
    METHOD checkFactoryCalendarId.
      SELECT FactoryCalendarID FROM I_FactoryCalendarBasic INTO TABLE @DATA(lt_fcal).
      READ ENTITIES OF zr_factoryorder_### IN LOCAL MODE
        ENTITY FactoryOrder
        FIELDS ( FactoryCalendar )
        WITH CORRESPONDING #( keys )
        RESULT DATA(lt_orders).

      LOOP AT lt_orders INTO DATA(ls_order).
        READ TABLE lt_fcal WITH KEY FactoryCalendarID = ls_order-FactoryCalendar INTO DATA(ls_fcal).
        IF ls_fcal IS INITIAL.
          APPEND VALUE #( %tky = ls_order-%tky ) TO failed-factoryorder.
          APPEND VALUE #( %tky = ls_order-%tky
          %msg = new_message_with_text(
          severity = if_abap_behv_message=>severity-error
          text = 'Factory calendar does not exist.'
          ) ) TO reported-factoryorder.
        ENDIF.
      ENDLOOP.
    ENDMETHOD.
    ```

6. Save and activate your changes.

7. Reload the service binding **Preview**. Create a new factory order and input a non-existent factory calendar ID. Check that the new validation is triggered when clicking **Save**.
<!-- border -->
![Factory calenddar does not exist](screenshot_step5.png)
The validation starts by reading all available factory calendar IDs from the released CDS view `I_FactoryCalendarBasic`. Additionally, the factory calendar ID of the entries for which the validation is run are also read. For each entry, the existence of the calendar is checked by filtering the first table for the chosen calendar ID. If the result is empty, an error is raised.

### Create a Scalar Function Definition

Each factory order receives a production start date and the required production days as input. The chosen factory calendar, which defines the relevant working days, can then be used to determine the correct production end date. This can be implemented by using a CDS scalar function, which dynamically performs the calculation for each order, to add the result to your CDS model. The first step is to create a scalar function definition.

1. Right-click on your ABAP package and select **New > Other ABAP Repository Object** from the context menu.

2. Search for **Scalar Function Definition** and click **Next**.

3. Maintain the required information:
    <ol type="a"><li>Name: `ZFACTORYORDER_SCALAR_###`
    </li><li>Description: `Factory Calendar Tutorial – Scalar Function Definition`</li></ol>
    Click **Next**.
4. Select a transport request and click **Next**.
5. Choose template `defineScalarFunction` and click **Finish**.
6. Replace the generated code with the following code:

    ```ABAP
    define scalar function ZFACTORYORDER_SCALAR_###
    with parameters
    i_calendar_id: abap.char( 32 ),
    i_production_start_date: abap.dats,
    i_days_to_produce : abap.int4
    returns abap.dats
    ```

7. Save and activate your changes.

### Create a Scalar Function Implementation Reference

To link the scalar function definition to the actual logic a scalar function implementation reference object must be created. The implementation reference will point to the class and method containing the function logic, which will be created in the next step.

1. Right-click on your ABAP package and select **New > Other ABAP Repository Object** from the context menu.

2. Search for **Scalar Function Implementation Reference** and click **Next**.

3. Maintain the required information:
    <ol type="a"><li>Name: `ZFACTORYORDER_SCALAR_###_SQL`
    </li><li>Description: `Factory Calendar Tutorial – Scalar Function Impl. Ref.`</li></ol>
    Click **Next**.

4. Select a transport request and click **Finish**.

5. Maintain `ZCL_FACTORYORDER_###=>CALCULATE_PRODUCTION_END_DATE` for the **AMDP Reference** field.

6. Save the object. Activation is not yet possible, as the implementation class does not exist yet.

The suffix `_SQL` indicates that the scalar function is implemented using SQL. The rest of the name must be identical to an existing scalar function definition.

### Implement the Logic for the Scalar Function

You will now implement the actual logic for the scalar function, which will be contained in an ABAP class. A special syntax is used for methods which implement scalar functions, as can be seen below.

1. Right-click on your ABAP package and select **New > ABAP Class** from the context menu.

2. Maintain the required information:
    <ol type="a"><li>Name: `ZCL_FACTORYORDER_###`
    </li><li>Description: `Factory Calendar Tutorial – Production Date calculation`
    </li><li>Add interface `if_amdp_marker_hdb`</li></ol>
    Click **Next**.

3. Select a transport request and click **Finish**.

4. Declare and implement a new class method as shown in the code snippet below:

    ```ABAP
    CLASS zcl_factoryorder_### DEFINITION
      PUBLIC
      FINAL
      CREATE PUBLIC .

      PUBLIC SECTION.
        INTERFACES if_amdp_marker_hdb .
        CLASS-METHODS calculate_production_end_date FOR SCALAR FUNCTION zfactoryorder_scalar_###.
      PROTECTED SECTION.
      PRIVATE SECTION.
    ENDCLASS.

    CLASS zcl_factoryorder_### IMPLEMENTATION.
      METHOD calculate_production_end_date BY DATABASE FUNCTION
                    FOR HDB
                    LANGUAGE SQLSCRIPT
                    OPTIONS READ-ONLY.
        SELECT to_dats( add_workdays(i_calendar_id, i_production_start_date, i_days_to_produce) ) INTO result FROM dummy;
      ENDMETHOD.
    ENDCLASS.
    ```

5. Save the object.

6. Use the **Activate inactive ABAP development objects** functionality (`Crtl` + `Shift` + `F3`) to activate all class artifacts and the scalar function implementation reference. Since the objects are dependent on each other, they must be activated simultaneously.

Two built-in HANA SQL functions are used. First, `add_workdays` is used to add the specified days to the production start date. This function makes use of the factory calendars maintained in your system. Since the output is in **DATE** format, function `to_dats` is used to convert it to the same ABAP date format as the other dates.

### Consume the Scalar Function in your Business Object

You can now use the defined scalar function to add a production date field to your CDS model.

1. Open the interface layer CDS view `ZR_FACTORYORDER_###`.
2. Add a new field which is calculated using the scalar function and the other required fields as input:

    ```ABAP
    @AccessControl.authorizationCheck: #CHECK
    @EndUserText.label: '##GENERATED Factory Calendar Tutorial'
    define root view entity ZR_FACTORYORDER_###
      as select from zfactorycal_### as FactoryOrder
    {
      key order_id as OrderID,
      description as Description,
      days_to_produce as DaysToProduce,
      factory_calendar as FactoryCalendar,
      production_start_date as OrderDate,
      delivery_date as DeliveryDate,
      @Semantics.user.createdBy: true
      created_by as CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      created_at as CreatedAt,
      @Semantics.user.lastChangedBy: true
      last_changed_by as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      last_changed_at as LastChangedAt,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      local_last_changed_at as LocalLastChangedAt,
      ZFACTORYORDER_SCALAR_###( i_calendar_id =>  factory_calendar, i_production_start_date => production_start_date, i_days_to_produce => days_to_produce) as ProductionEndDate

    }
    ```

3. Save and activate the changes.

4. Open the interface layer behavior definition `ZR_FACTORYORDER_###`.

5. You should see a syntax error appear in your behavior definition. Since you added a field to the CDS view, your draft table does no longer match the data model. Use the offered quick fix to adjust the draft table.

6. Save and activate the draft table.

7. Back in the behavior definition, add the new field `productionenddate` to the group of **read-only** fields.

8. Save and activate the behavior definition.

9. Open the projection layer CDS view `ZC_FACTORYORDER_###`.

10. Expose the new field `ProductionEndDate` by adding it to the CDS view.

11. Save and activate the changes.

12. Open the metadata extension `ZC_FACTORYORDER_###`.

13. Add UI annotations for the newly defined field, so that it will be visible on the UI:

    ```ABAP
    @UI.lineItem: [ {
      position: 60 , 
      importance: #MEDIUM, 
      label: 'ProductionEndDate'
    } ]
    @UI.identification: [ {
      position: 60 , 
      label: 'ProductionEndDate'
    } ]
    ProductionEndDate;
    ```

14. Save and activate the changes.
15. Reload the service binding **Preview**. Check that the new field is visible in the UI, and that it is automatically calculated once a factory order is saved.

<!-- border -->
![Preview](screenshot_step9.png)

### Define a Validation using the chosen Factory Calendar

The user inputs a delivery date when creating an order. This date should be validated to ensure it is not earlier than the production end date that is calculated by the system. You can achieve this by introducing a validation in your business object.

1. Open the interface layer behavior definition `ZR_FACTORYORDER_###`.

2. Define a new validation on the delivery date field, which shall be triggered during the save process. Add the new validation to the standard **Prepare** action:

    ```ABAP
    validation checkFactoryCalendarId on save { field FactoryCalendar; }
    validation checkDeliveryDate on save { field DeliveryDate; }
    draft determine action Prepare { validation checkFactoryCalendarId; validation checkDeliveryDate; }
    ```

3. Save and activate your changes.
4. Use the quick fix that is offered to generate the required method shell in your behavior implementation class. The IDE will automatically navigate to the class.
5. Implement the method as shown below:

    ```ABAP
    METHOD checkDeliveryDate.

      READ ENTITIES OF zr_factoryorder_### IN LOCAL MODE
                ENTITY FactoryOrder
                  ALL FIELDS
                    WITH CORRESPONDING #( keys )
                RESULT DATA(lt_orders).

      LOOP AT lt_orders INTO DATA(ls_order).
        TRY.
            DATA(lo_fcal_run) = cl_fhc_calendar_runtime=>create_factorycalendar_runtime(
                iv_factorycalendar_id = ls_order-FactoryCalendar ).

            DATA(production_end_date) = lo_fcal_run->add_workingdays_to_date(
                                      iv_start                 = ls_order-ProductionStartDate
                                      iv_number_of_workingdays = ls_order-DaysToProduce
                                    ).
          CATCH cx_fhc_runtime INTO DATA(lx_err).
            "Exception handling – here you can catch any errors related to the factory calendar runtime
        "This is out of scope for this tutorial 
        ENDTRY.
        IF ls_order-DeliveryDate < production_end_date.
          APPEND VALUE #( %tky =  ls_order-%tky ) TO failed-factoryorder.
          APPEND VALUE #( %tky =  ls_order-%tky
                          %msg =  new_message_with_text(
                                    severity = if_abap_behv_message=>severity-error
                                    text     = 'Delivery date is too early.'
                                  ) ) TO reported-factoryorder.
        ENDIF.
      ENDLOOP.
      
    ENDMETHOD.
    ```

6. Save and activate your changes.
7. Reload the service binding **Preview**. Create a new factory order and input a delivery date that is too early (for example, before the production start date). Check that the new validation is triggered when clicking **Save**.

<!-- border -->
![Preview](screenshot_step10.png)

The validation starts by reading all fields of the entries for which the validation is run. For each entry, the chosen factory calendar ID is used to create a factory calendar runtime object. This object is then used to compute the production end date, using the start date and required production days. This date is compared to the delivery date and an error is raised if the latter is too early.

You could also compare the delivery date to the already computed production end date field. However, since the goal is to showcase the factory calendar reuse functionality, the validation is implemented using the **`cl_fhc_calendar_runtime`** class and related released objects.

### Summary

With that, you have successfully implemented all the planned features of your application. You have used factory calendar functionality in your business scenario and you have also learned how to define and consume CDS scalar functions to enrich your CDS models.

When creating a factory order, the application...

- Offers a value help showing all available factory calendars,

- Validates the existence of the specified factory calendar,

- Calculates the production end date automatically using the provided input,

- Validates the delivery date, making sure that it is not before the calculated production end date.

### Test yourself

---
