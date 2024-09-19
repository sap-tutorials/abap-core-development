---
parser: v2
auto_validation: true
primary_tag: software-product-function>sap-s-4hana-cloud--abap-environment
tags:  [ tutorial>beginner, software-product-function>sap-s-4hana-cloud--abap-environment, programming-tool>abap-development, programming-tool>abap-extensibility]
time: 25
author_name: Merve Temel
author_profile: https://github.com/mervey45
---
  
# Create Value Help, Enhance the Behavior Definition and Behavior Implementation of the Shopping Cart Business Object
<!-- description --> Create your first order in the shopping cart.

In the shopping cart, customers can order various items. Once an item is ordered, a new purchase requisition is created via purchase requisitions API.

 
## Prerequisites  
- This tutorial can be used in both SAP S/4HANA Cloud, private edition system and SAP S/4HANA on-premise system with release 2022 FPS01. We suggest using a [Fully-Activated Appliance] (https://blogs.sap.com/2018/12/12/sap-s4hana-fully-activated-appliance-create-your-sap-s4hana-1809-system-in-a-fraction-of-the-usual-setup-time/) in SAP Cloud Appliance Library for an easy start without the need for system setup.
- For SAP S/4HANA on-premise, create developer user with full development authorization 
- You have installed the latest [Eclipse with ADT](abap-install-adt).
- Business role `SAP_BR_PURCHASER` needs to be assigned to your business user
- Use Starter Development Tenant in S/4HANA Cloud for the tutorial to have necessary sample data in place. See [3-System Landscape and Transport Management](https://help.sap.com/docs/SAP_S4HANA_CLOUD/a630d57fc5004c6383e7a81efee7a8bb/e022623ec1fc4d61abb398e411670200.html?state=DRAFT&version=2208.503).


## You will learn  
- How to enhance a behavior definition
- How to create a value help
- How to enhance a behavior implementation
- How to run the SAP Fiori Elements Preview

## Intro
In this tutorial, wherever ### appears, use a number (e.g. 000). This tutorial is done with the placeholder 000.

---

### Enhance behavior definition of data model

**In this tutorial example a SAP S/4HANA Cloud, ABAP environment system was used. The mode therefore is `strict (2)`.**

  1. Open your behavior definition **`ZR_SHOPCARTTP_###`** to enhance it. Add the following statements to your behavior definition:

    ```ABAP
    update (features: instance);
    .
    .
    draft action(features: instance) Edit;
    ```

     ![projection](updatenew.png)

  2. Replace the following statements to your behavior definition:

    ```ABAP
    draft determine action Prepare { validation checkOrderedQuantity;  validation checkDeliveryDate;}
        determination setInitialOrderValues on modify { create; }
        determination calculateTotalPrice on modify { create; field Price; } 
      validation checkOrderedQuantity on save { create; field OrderQuantity; }
      validation checkDeliveryDate on save { create; field DeliveryDate; }
    ```
   
     ![projection](bdef5xx.png) 
 
  3. Check your behavior definition:

    ```ABAP
    managed implementation in class ZBP_SHOPCARTTP_### unique;
    strict ( 2 );
    with draft;

    define behavior for ZR_SHOPCARTTP_### alias ShoppingCart
    persistent table zashopcart_###
    draft table ZDSHOPCART_###
    etag master LocalLastChangedAt
    lock master total etag LastChangedAt
    authorization master( global )
    {
      field ( readonly ) 
      OrderUUID,
      CreatedAt,
      CreatedBy,
      LastChangedAt,
      LastChangedBy,
      LocalLastChangedAt,
      PurchaseRequisition,
      PrCreationDate,
      DeliveryDate;

      field ( numbering : managed )
      OrderUUID;

      create;
      update(features: instance) ;
      delete;

      draft action(features: instance) Edit;
      draft action Activate;
      draft action Discard; 
      draft action Resume;
      draft determine action Prepare { validation checkOrderedQuantity;  validation checkDeliveryDate;}
        determination setInitialOrderValues on modify { create; }
        determination calculateTotalPrice on modify { create; field Price; }
      validation checkOrderedQuantity on save { create; field OrderQuantity; }
      validation checkDeliveryDate on save { create; field DeliveryDate; }

      mapping for ZASHOPCART_### 
      {
        OrderUUID = order_uuid;
        OrderID = order_id;
        OrderedItem = ordered_item;
        Price = price;
        TotalPrice = total_price;
        Currency = currency;
        OrderQuantity = order_quantity;
        DeliveryDate = delivery_date;
        OverallStatus = overall_status;
        Notes = notes;
        CreatedBy = created_by;
        CreatedAt = created_at;
        LastChangedBy = last_changed_by;
        LastChangedAt = last_changed_at;
        LocalLastChangedAt = local_last_changed_at;
        PurchaseRequisition = purchase_requisition;
        PrCreationDate = pr_creation_date;
      }
    }
    ```
    **Hint:** Please replace **`###`** with your ID.
    
   4. Save and activate. 


### Create data definition for products

This data definition is needed to create a value help for products.

 1. Right-click **Data Definitions** and select **New Data Definition**.
  
    ![projection](products.png) 

 2. Create a new data definition:
    - Name: `ZI_Products_###`
    - Description: data definition for products
  
      Click **Next >**.

      ![projection](products2.png) 
  
 3. Click **Finish**.  
   
      ![projection](products3.png) 

 4. In your data definition **`ZI_Products_###`** replace your code with following:

    ```ABAP
    @AbapCatalog.viewEnhancementCategory: [#NONE]
    @AccessControl.authorizationCheck: #NOT_REQUIRED
    @EndUserText.label: 'Value Help for I_PRODUCT'
    @Metadata.ignorePropagatedAnnotations: true
    @ObjectModel.usageType:{
      serviceQuality: #X,
      sizeCategory: #S,
      dataClass: #MIXED
    }
    define view entity ZI_PRODUCTS_###
      as select from I_Product
    {
      key Product                                                 as Product,
          _Text[1: Language=$session.system_language].ProductName as ProductText,
          @Semantics.amount.currencyCode: 'Currency'
          case
            when Product = 'D001' then cast ( 1000.00 as abap.dec(16,2) ) 
            when Product = 'D002' then cast ( 499.00 as abap.dec(16,2) ) 
            when Product = 'D003' then cast ( 799.00 as abap.dec(16,2) ) 
            when Product = 'D004' then cast ( 249.00 as abap.dec(16,2) )
            when Product = 'D005' then cast ( 1500.00 as abap.dec(16,2) ) 
            when Product = 'D006' then cast ( 30.00 as abap.dec(16,2) ) 
            else cast ( 100000.00 as abap.dec(16,2) ) 
          end                                                     as Price,
          
          @UI.hidden: true
          cast ( 'EUR' as abap.cuky( 5 ) )                        as Currency,

          @UI.hidden: true
          ProductGroup                                            as ProductGroup,

          @UI.hidden: true
          BaseUnit                                                as BaseUnit

    }
    where
        Product = 'D001'
      or Product = 'D002'
      or Product = 'D003'
      or Product = 'D004'
      or Product = 'D005'
      or Product = 'D006'
    ```

 5. Save and activate.


### Enhance metadata extension

 1. In your metadata extension **`ZC_SHOPCARTTP_###`** replace your code with following:

    ```ABAP
    @Metadata.layer: #CORE
    @UI: {
      headerInfo: {
        typeName: 'ShoppingCart', 
        typeNamePlural: 'ShoppingCarts'
      , title: {
          type: #STANDARD,
          label: 'ShoppingCart',
          value: 'OrderID'
        }
      },
      presentationVariant: [ {
        sortOrder: [ {
          by: 'OrderID',
          direction: #DESC
        } ],
        visualizations: [ {
          type: #AS_LINEITEM
        } ]
      } ]
    }
    annotate view ZC_SHOPCARTTP_### with
    {
      @UI.facet: [ {
        id: 'idIdentification', 
        type: #IDENTIFICATION_REFERENCE, 
        label: 'ShoppingCart', 
        position: 10 
      } ]
      @UI.hidden: true
      orderuuid;
      
      @UI.lineItem: [ {
        position: 20 ,
        importance: #HIGH,
        label: 'OrderID'
      } ] 
      @UI.identification: [ {
        position: 20 ,
        label: 'OrderID'
      } ]
      @UI.selectionField: [ {
        position: 20
      } ]
      orderid;
      @Consumption.valueHelpDefinition: [{ entity: 
                    {name: 'ZI_PRODUCTS_###' , element: 'ProductText' },
                    additionalBinding: [{ localElement: 'price', element: 'Price', usage: #RESULT },
                                        { localElement: 'currency', element: 'Currency', usage: #RESULT }
                                                                          ]
                    }]
      
      @UI.lineItem: [ {
        position: 30 ,
        importance: #HIGH,
        label: 'Ordered Item'
      } ]
      @UI.identification: [ {
        position: 30 ,
        label: 'Ordered Item'
      } ]
      @UI.selectionField: [ {
        position: 30
      } ]
      ordereditem;
      
      @UI.lineItem: [ {
        position: 40 ,
        importance: #HIGH,
        label: 'Price'
      } ]
      @UI.identification: [ {
        position: 40 ,
        label: 'Price'
      } ]
      @UI.selectionField: [ {
        position: 40
      } ]
      price;
      
      @UI.lineItem: [ {
        position: 45 ,
        importance: #HIGH,
        label: 'Total Price'
      } ]
      @UI.identification: [ {
        position: 45 ,
        label: 'Total Price'
      } ]
      @UI.selectionField: [ {
        position: 50
      } ]
      totalprice;
      @Consumption.valueHelpDefinition: [ { entity: { name: 'I_Currency', element: 'Currency' } } ]
      @UI.lineItem: [ {
        position: 50 ,
        importance: #HIGH,
        label: 'currency'
      } ]
      @UI.identification: [ {
        position: 50 ,
        label: 'Currency'
      } ]
      @UI.selectionField: [ {
        position: 60
      } ]
      currency;
      
      @UI.lineItem: [ {
        position: 55 ,
        importance: #HIGH,
        label: 'Ordered Quantity'
      } ]
      @UI.identification: [ {
        position: 55 ,
        label: 'Ordered Quantity'
      } ]
      @UI.selectionField: [ {
        position: 65
      } ]
      orderquantity;
      
      @UI.lineItem: [ {
        position: 60 ,
        importance: #HIGH,
        label: 'Delivery Date'
      } ]
      @UI.identification: [ {
        position: 60 ,
        label: 'Delivery Date'
      } ]
      deliverydate;
      @UI.lineItem: [ {
        position: 65 ,
        importance: #HIGH,
        label: 'Overall Status'
      } ]
      @UI.identification: [ {
        position: 65 ,
        label: 'Overall Status'
      } ]
      overallstatus;
    @UI.lineItem: [ {
        position: 70 ,
        importance: #HIGH,
        label: 'Notes'
      } ]
      @UI.identification: [ {
        position: 70 ,
        label: 'Notes'
      } ]
      notes;
      
      @UI.hidden: true
      locallastchangedat;
      
      @UI.lineItem: [ {
        position: 70 ,
        label: 'Purchase requisition number',
        importance: #HIGH
      }]
      purchaserequisition;
      
      @UI.lineItem: [ {
        position: 75 ,
        importance: #HIGH,
        label: 'PR Creation Date'
      } ]
      @UI.identification: [ {
        position: 75 ,
        label: 'PR Creation Date'
      } ]
      prcreationdate;
    }
    ```
    
 2. Save and activate.
   
### Enhance behavior implementation

**Hint:** Please replace **`###`** with your ID. 

  1. Open the behavior implementation **`ZBP_SHOPCARTTP_###`**, add the constant `c_overall_status` to your behavior implementation. In your **Local Types**, replace your code with following:

    ```ABAP
    CLASS lhc_shopcart DEFINITION INHERITING FROM cl_abap_behavior_handler.
      PRIVATE SECTION.
        CONSTANTS:
          BEGIN OF c_overall_status,
            new       TYPE string VALUE 'New / Composing',
    *        composing  TYPE string VALUE 'Composing...',
            submitted TYPE string VALUE 'Submitted / Approved',
            cancelled TYPE string VALUE 'Cancelled',
          END OF c_overall_status.
        METHODS:
          get_global_authorizations FOR GLOBAL AUTHORIZATION
            IMPORTING
            REQUEST requested_authorizations FOR ShoppingCart
            RESULT result,
          get_instance_features FOR INSTANCE FEATURES
            IMPORTING keys REQUEST requested_features FOR ShoppingCart RESULT result.

        METHODS calculateTotalPrice FOR DETERMINE ON MODIFY
          IMPORTING keys FOR ShoppingCart~calculateTotalPrice.

        METHODS setInitialOrderValues FOR DETERMINE ON MODIFY
          IMPORTING keys FOR ShoppingCart~setInitialOrderValues.

        METHODS checkDeliveryDate FOR VALIDATE ON SAVE
          IMPORTING keys FOR ShoppingCart~checkDeliveryDate.

        METHODS checkOrderedQuantity FOR VALIDATE ON SAVE
          IMPORTING keys FOR ShoppingCart~checkOrderedQuantity.
    ENDCLASS.

    CLASS lhc_shopcart IMPLEMENTATION.
      METHOD get_global_authorizations.
      ENDMETHOD.
      METHOD get_instance_features.

        " read relevant olineShop instance data
        READ ENTITIES OF zr_shopcarttp_### IN LOCAL MODE
          ENTITY ShoppingCart
            FIELDS ( OverallStatus )
            WITH CORRESPONDING #( keys )
          RESULT DATA(OnlineOrders)
          FAILED failed.

        " evaluate condition, set operation state, and set result parameter
        " update and checkout shall not be allowed as soon as purchase requisition has been created
        result = VALUE #( FOR OnlineOrder IN OnlineOrders
                          ( %tky                   = OnlineOrder-%tky
                            %features-%update
                              = COND #( WHEN OnlineOrder-OverallStatus = c_overall_status-submitted  THEN if_abap_behv=>fc-o-disabled
                                        WHEN OnlineOrder-OverallStatus = c_overall_status-cancelled THEN if_abap_behv=>fc-o-disabled
                                        ELSE if_abap_behv=>fc-o-enabled   )
    *                         %features-%delete
    *                           = COND #( WHEN OnlineOrder-PurchaseRequisition IS NOT INITIAL THEN if_abap_behv=>fc-o-disabled
    *                                     WHEN OnlineOrder-OverallStatus = c_overall_status-cancelled THEN if_abap_behv=>fc-o-disabled
    *                                     ELSE if_abap_behv=>fc-o-enabled   )
                            %action-Edit
                              = COND #( WHEN OnlineOrder-OverallStatus = c_overall_status-submitted THEN if_abap_behv=>fc-o-disabled
                                        WHEN OnlineOrder-OverallStatus = c_overall_status-cancelled THEN if_abap_behv=>fc-o-disabled
                                        ELSE if_abap_behv=>fc-o-enabled   )

                            ) ).
      ENDMETHOD.

      METHOD calculateTotalPrice.
        DATA total_price TYPE zr_shopcarttp_###-TotalPrice.

        " read transfered instances
        READ ENTITIES OF zr_shopcarttp_### IN LOCAL MODE
          ENTITY ShoppingCart
            FIELDS ( OrderID TotalPrice )
            WITH CORRESPONDING #( keys )
          RESULT DATA(OnlineOrders).

        LOOP AT OnlineOrders ASSIGNING FIELD-SYMBOL(<OnlineOrder>).
          " calculate total value
          <OnlineOrder>-TotalPrice = <OnlineOrder>-Price * <OnlineOrder>-OrderQuantity.
        ENDLOOP.

        "update instances
        MODIFY ENTITIES OF zr_shopcarttp_### IN LOCAL MODE
          ENTITY ShoppingCart
            UPDATE FIELDS ( TotalPrice )
            WITH VALUE #( FOR OnlineOrder IN OnlineOrders (
                              %tky       = OnlineOrder-%tky
                              TotalPrice = <OnlineOrder>-TotalPrice
                            ) ).
      ENDMETHOD.

      METHOD setInitialOrderValues.

        DATA delivery_date TYPE I_PurchaseReqnItemTP-DeliveryDate.
        DATA(creation_date) = cl_abap_context_info=>get_system_date(  ).
        "set delivery date proposal
        delivery_date = cl_abap_context_info=>get_system_date(  ) + 14.
        "read transfered instances
        READ ENTITIES OF ZR_shopcarttp_### IN LOCAL MODE
          ENTITY ShoppingCart
            FIELDS ( OrderID OverallStatus  DeliveryDate )
            WITH CORRESPONDING #( keys )
          RESULT DATA(OnlineOrders).

        "delete entries with assigned order ID
        DELETE OnlineOrders WHERE OrderID IS NOT INITIAL.
        CHECK OnlineOrders IS NOT INITIAL.

        " **Dummy logic to determine order IDs**
        " get max order ID from the relevant active and draft table entries
        SELECT MAX( order_id ) FROM zashopcart_### INTO @DATA(max_order_id). "active table
        SELECT SINGLE FROM zdshopcart_### FIELDS MAX( orderid ) INTO @DATA(max_orderid_draft). "draft table
        IF max_orderid_draft > max_order_id.
          max_order_id = max_orderid_draft.
        ENDIF.

        "set initial values of new instances
        MODIFY ENTITIES OF ZR_SHOPCARTTP_### IN LOCAL MODE
          ENTITY ShoppingCart
            UPDATE FIELDS ( OrderID OverallStatus  DeliveryDate Price  )
            WITH VALUE #( FOR order IN OnlineOrders INDEX INTO i (
                              %tky          = order-%tky
                              OrderID       = max_order_id + i
                              OverallStatus = c_overall_status-new  "'New / Composing'
                              DeliveryDate  = delivery_date
                              CreatedAt     = creation_date
                            ) ).
        .
      ENDMETHOD.

      METHOD checkDeliveryDate.

    *   " read transfered instances
        READ ENTITIES OF zr_shopcarttp_### IN LOCAL MODE
          ENTITY ShoppingCart
            FIELDS ( DeliveryDate )
            WITH CORRESPONDING #( keys )
          RESULT DATA(OnlineOrders).

        DATA(creation_date) = cl_abap_context_info=>get_system_date(  ).
        "raise msg if 0 > qty <= 10
        LOOP AT OnlineOrders INTO DATA(online_order).


          IF online_order-DeliveryDate IS INITIAL OR online_order-DeliveryDate = ' '.
            APPEND VALUE #( %tky = online_order-%tky ) TO failed-ShoppingCart.
            APPEND VALUE #( %tky         = online_order-%tky
                            %state_area   = 'VALIDATE_DELIVERYDATE'
                            %msg          = new_message_with_text(
                                    severity = if_abap_behv_message=>severity-error
                                    text     = 'Delivery Date cannot be initial' )
                          ) TO reported-ShoppingCart.

          ELSEIF  ( ( online_order-DeliveryDate ) - creation_date ) < 14.
            APPEND VALUE #(  %tky = online_order-%tky ) TO failed-ShoppingCart.
            APPEND VALUE #(  %tky          = online_order-%tky
                            %state_area   = 'VALIDATE_DELIVERYDATE'
                            %msg          = new_message_with_text(
                                    severity = if_abap_behv_message=>severity-error
                                    text     = 'Delivery Date should be atleast 14 days after the creation date'  )

                            %element-orderquantity  = if_abap_behv=>mk-on
                          ) TO reported-ShoppingCart.
          ENDIF.
        ENDLOOP.
      ENDMETHOD.

      METHOD checkOrderedQuantity.

        "read relevant order instance data
        READ ENTITIES OF zr_shopcarttp_### IN LOCAL MODE
        ENTITY ShoppingCart
        FIELDS ( OrderID OrderedItem OrderQuantity )
        WITH CORRESPONDING #( keys )
        RESULT DATA(OnlineOrders).

        "raise msg if 0 > qty <= 10
        LOOP AT OnlineOrders INTO DATA(OnlineOrder).
          APPEND VALUE #(  %tky           = OnlineOrder-%tky
                          %state_area    = 'VALIDATE_QUANTITY'
                        ) TO reported-ShoppingCart.

          IF OnlineOrder-OrderQuantity IS INITIAL OR OnlineOrder-OrderQuantity = ' '.
            APPEND VALUE #( %tky = OnlineOrder-%tky ) TO failed-ShoppingCart.
            APPEND VALUE #( %tky          = OnlineOrder-%tky
                            %state_area   = 'VALIDATE_QUANTITY'
                            %msg          = new_message_with_text(
                                    severity = if_abap_behv_message=>severity-error
                                    text     = 'Quantity cannot be empty' )
                            %element-orderquantity = if_abap_behv=>mk-on
                          ) TO reported-ShoppingCart.

          ELSEIF OnlineOrder-OrderQuantity > 10.
            APPEND VALUE #(  %tky = OnlineOrder-%tky ) TO failed-ShoppingCart.
            APPEND VALUE #(  %tky          = OnlineOrder-%tky
                            %state_area   = 'VALIDATE_QUANTITY'
                            %msg          = new_message_with_text(
                                    severity = if_abap_behv_message=>severity-error
                                    text     = 'Quantity should be below 10' )

                            %element-orderquantity  = if_abap_behv=>mk-on
                          ) TO reported-ShoppingCart.
          ENDIF.
        ENDLOOP.
      ENDMETHOD.
    ENDCLASS.
    ```

   2. Save and activate.

   3. Go back to your behavior definition `ZR_SHOPCARTTP_###` and activate it again, if needed. 


### Run SAP Fiori Elements preview and create first order

 1. Select **`ShoppingCart`** in your service binding and click **Preview** to open SAP Fiori Elements preview.

     ![preview](uinew0.png)

 2. Click **Create** to create a new entry.

     ![preview](order.png)

 3. Make use of the value help for ordered item and select one. Add also the ordered quantity and click **Create**.

     ![preview](order2.png)

 4. Your order is now created and the total price is calculated automatically.

     ![preview](order3.png)

 
### Test yourself
  