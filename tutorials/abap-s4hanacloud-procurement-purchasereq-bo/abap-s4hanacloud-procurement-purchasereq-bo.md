---
parser: v2
auto_validation: true
primary_tag: software-product-function>sap-s-4hana-cloud--abap-environment
tags:  [ tutorial>beginner, software-product>sap-btp--abap-environment, software-product-function>s-4hana-cloud-abap-environment, programming-tool>abap-development, programming-tool>abap-extensibility]
time: 25
author_name: Merve Temel
author_profile: https://github.com/mervey45
---

# Create an Online Shop Business Object
<!-- description --> Create an online shop business object with SAP S/4HANA Cloud ABAP Environment or SAP S/4HANA on-premise.

 
## Prerequisites  
- This tutorial can be used in both SAP S/4HANA Cloud, private edition system and SAP S/4HANA on-premise system with release 2022 FPS01. We suggest using a [Fully-Activated Appliance] (https://blogs.sap.com/2018/12/12/sap-s4hana-fully-activated-appliance-create-your-sap-s4hana-1809-system-in-a-fraction-of-the-usual-setup-time/) in SAP Cloud Appliance Library for an easy start without the need for system setup.
- For SAP S/4HANA on-premise, create developer user with full development authorization 
- You have installed the latest [Eclipse with ADT](abap-install-adt).
- Business role `SAP_BR_PURCHASER` needs to be assign to your business user
- Use Starter Development Tenant in S/4HANA Cloud for the tutorial to have necessary sample data in place. See [3-System Landscape and Transport Management](https://help.sap.com/docs/SAP_S4HANA_CLOUD/a630d57fc5004c6383e7a81efee7a8bb/e022623ec1fc4d61abb398e411670200.html?state=DRAFT&version=2208.503).
- You have imported the [SAP Note 3330593](https://launchpad.support.sap.com/#/notes/3330593) into your system.
- You have imported the [SAP Note 3280851](https://me.sap.com/notes/3280851) into your system.
- Business role `SAP_BR_PURCHASER` needs to be assigned to your business user

## You will learn  
- How to logon to SAP S/4HANA Cloud ABAP Environment
- How to logon to SAP S/4HANA on-premise
- How to create an ABAP package
- How to create a database table
- How to create transactional UI services
- How to create structure
- How to enhance behavior definition
- How to publish service binding
- How to run SAP Fiori Elements Preview
 
## Intro
In this tutorial, wherever X/XXX/#/### appears, use a number (e.g. 000).

---

### Alternative 1 - Logon to SAP S/4HANA Cloud ABAP Environment

  1. Open Eclipse, select **File** > **New** > **Other**.

      ![logon](logon.png)

  2. Search **ABAP Cloud Project**, select it and click **Next >**.

      ![logon](logon2.png)

  3. Select **SAP S/4HANA Cloud ABAP Environment**, enter the ABAP service instance URL and click **Next >**.

      ![logon](logon3.png)

  4. Click **Open Logon Page in Browser** to log in.

      ![logon](logon4.png)

      **HINT**: The administrator receives an welcome e-mail after provisioning. This e-mail includes the system URL. By removing `/ui` you can log into the SAP S/4HANA Cloud ABAP Environment system. Further information can be found [here](https://help.sap.com/docs/SAP_S4HANA_CLOUD/6aa39f1ac05441e5a23f484f31e477e7/4b962c243a3342189f8af460cc444883.html?locale=en-US&state=DRAFT).

  5. Click **Next >**.

      ![logon](logon5.png)

  6. Check your ABAP service instance connection and click **Finish**.

      ![logon](logon6.png)

  7. Now your project will be available in the project explorer.

      ![logon](logon7.png)

     Now move on with step 3.

### Alternative 2 - Logon to SAP S/4HANA on-premise

**For S/4HANA on premise:**
  - Remark that SAP S/4HANA 2022 `(FPS01)` [Fully-Activated Appliance](https://blogs.sap.com/2018/12/12/sap-s4hana-fully-activated-appliance-create-your-sap-s4hana-1809-system-in-a-fraction-of-the-usual-setup-time/) can be used.
  - The steps for fully-Qualified Domain Name & SSL Certificates can be found [here](https://www.sap.com/documents/2020/06/109b1be0-9e7d-0010-87a3-c30de2ffd8ff.html).

**Hint:** Remark for Set Up Developer Extensibility, if CAL instance is not used but bare-minimum [S/4HANA on-premise](https://help.sap.com/docs/ABAP_PLATFORM_NEW/ed0e11412f9841e7ac5cd9a6799368d4/31367ef6c3e947059e0d7c1cbfcaae93.html?state=DRAFT&version=202210.latest). Please follow these steps, if you don't use a CAL instance.
- For SAP S/4HANA on-premise, create developer user with full development authorization.


  1. Open Eclipse, select **File** > **New** and **ABAP Project**.

      ![logon](alternative.png)

  2. In the System Connection dialogue click on the link **new system connection**.

      ![logon](alternative2.png)

  3. In the Connection Settings dialogue choose the connection type Custom Application Server. Then enter the following connection information:

      - System ID: `S4H`
      - Application Server: `xxx.xxx.xxx.xxx`
      - Instance Number: 00
      - and **deactivate** the check box **Activate Secure Network Communication (SNC)**

      Click **Next >**.

      ![logon](alternative3.png)

  4. Enter your credentials:
    
      - Client: `xxx`
      - User: `XXX`
      - Password: `xxxxxxxx`
      - Language: `xx`
      
      Click **Next >**.

      ![logon](alternative5.png)

  5. You can change the ABAP project name if you want. Click **Finish**.

      ![logon](alternative6.png)

  6. Add `ZLOCAL` to your favorites packages. Right click on **Favorite Packages** in your newly created ABAP project.

      ![logon](alternative7.png)

  7. Search for `ZLOCAL`. Select the entry and press **OK**.

      ![logon](alternative8.png)

     

### Create ABAP package

**Hint:** In case the CAL instance is not used, please set up for developer extensibility to get `ZLOCAL` package.
 - Create software component for local cloud-ready development with `ZLOCAL`
 - Create a structure package with `ZLOCAL`


  1.  Select **ZLOCAL** > **New** > **ABAP Package**.

      ![package](package.png)

  2.  Create new **ABAP package**:
       - Name: **`Z_PURCHASE_REQ_XXX`**
       - Description: Package XXX
       - Check **Add to favorite packages**

      ![package](package2.png)

       Click **Next >**.

  3. Click **Next >**.

      ![package](package3x.png)


  4.  Create a new request:
      -  Request Description: TR12345

      ![package](package3.png)

       Click **Finish**.


### Create database table

  1. Right-click your package `Z_PURCHASE_REQ_XXX` and select **New** > **Other ABAP Repository Object**.

      ![table](table.png)

  2. Search for **database table**, select it and click **Next >**.

      ![table](table2.png)

  3. Create new database table:
     - Name: `ZAONLINESHOP_XXX `
     - Description: Shop to purchase electronics

      ![table](table3.png)

       Click **Next >**.

  4. Click **Finish**.

      ![table](table4.png)

  5. Replace your code with following:

    ```ABAP
    @EndUserText.label : 'Shop to purchase electronics'
    @AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
    @AbapCatalog.tableCategory : #TRANSPARENT
    @AbapCatalog.deliveryClass : #A
    @AbapCatalog.dataMaintenance : #RESTRICTED
    define table zaonlineshop_xxx {

      key client            : abap.clnt not null;
      key order_uuid        : sysuuid_x16 not null;
      order_id              : abap.numc(8) not null;
      ordered_item          : abap.char(10) not null;
      @Semantics.amount.currencyCode : 'zaonlineshop_xxx.currency'
      price                 : abap.curr(11,2);
      @Semantics.amount.currencyCode : 'zaonlineshop_xxx.currency'
      total_price           : abap.curr(11,2);
      currency              : abap.cuky;
      order_quantity        : abap.numc(4);
      delivery_date         : abap.dats;
      overall_status        : abap.char(30);
      notes                 : abap.string(256);
      created_by            : abp_creation_user;
      created_at            : abp_creation_tstmpl;
      last_changed_by       : abp_lastchange_user;
      last_changed_at       : abp_lastchange_tstmpl;
      local_last_changed_at : abp_locinst_lastchange_tstmpl;
      purchase_requisition  : abap.char(10);
      pr_creation_date      : abap.dats;
    }
    ```

   6. Save and activate.


### Generate transactional UI services


  1. Right-click your database table `ZAONLINESHOP_XXX` and select **Generate ABAP Repository Objects**.

      ![cds](generate.png)

  2. Create new **ABAP repository object**:
     - Description: **Purchase Requisition XXX**
     - Generator: **ABAP RESTful Application Programming Model: UI Service**

      ![cds](generate2.png)

       Click **Next >**.

  3. Maintain the required information on the **Configure Generator** dialog to provide the name of your data model and generate them.         
     
     For that, navigate through the wizard tree **(Business Objects, Data Model, etc...)**, maintain the artefact names provided in the table below, and press **Next >**.

     Verify the maintained entries and press **Next >** to confirm. The needed artifacts will be generated.

     **Please note**: Error Invalid XML format.   
     If you receive an error message **Invalid XML format of the response**, this may be due to a bug in version 1.26 of the ADT tools. An update of your ADT plugin to version 1.26.3 will fix this issue.

    | **RAP Layer**                          | **Artefacts**           | **Artefact Names**                                  |
    |----------------------------------------|-------------------------|-----------------------------------------------------|
    | **Business Object**                    |                         |                                                     |
    |                                        | **Data Model**          | Data Definition Name: **`ZR_ONLINESHOPTP_###`**     |
    |                                        |                         | Alias Name: **`OnlineShop`**                        |  
    |                                        | **Behavior**            | Implementation Class: **`ZBP_ONLINESHOPTP_###`**    |
    |                                        |                         | Draft Table Name: **`ZDONLINESHOP_###`**            |  
    | **Service Projection (BO Projection)** |                         | Name: **`ZC_ONLINESHOPTP_###`**                     |
    | **Business Services**                  |                         |                                                     |
    |                                        | **Service Definition**  | Name: **`ZUI_ONLINESHOP_###`**                      |
    |                                        | **Service Binding**     | Name: **`ZUI_ONLINESHOP_O4_###`**                   |
    |                                        |                         | Binding Type: **`OData V4 - UI`**                   |

    ![cds](generate3.png)                    

    Click **Next >**.

  5. Click **Finish**.

      ![cds](generate4.png)



### Enhance behavior definition of data model

**Hint:** In case of S/4HANA 2022 `FPS01`, strict(1) mode must be used. 

**In this tutorial example, a SAP S/4HANA Cloud, ABAP environment system was used. The mode therefore is `strict (2)`.**
  
  1. Open your behavior definition **`ZR_ONLINESHOPTP_XXX`** to enhance it. Add the following read-only fields to your behavior definition:

    ```ABAP
    ,
    PurchaseRequisition,
    PrCreationDate,
    DeliveryDate;
    ```

     ![projection](bdef3x.png)


  2. Check your behavior definition:

    ```ABAP
    managed implementation in class ZBP_ONLINESHOPTP_XXX unique;
    strict ( 2 );
    with draft;

    define behavior for ZR_ONLINESHOPTP_XXX alias OnlineShop
    persistent table zaonlineshop_xxx
    draft table ZDONLINESHOP_XXX
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
      update;
      delete;

      draft action Edit;
      draft action Activate;
      draft action Discard;
      draft action Resume;
      draft determine action Prepare;

      mapping for ZAONLINESHOP_xxx  
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

   4. Save and activate.  

 
### Alternative Cloud -Publish service binding and run SAP Fiori Elements preview 

  1. Open your service binding **`ZUI_ONLINESHOP_O4_XXX`** and click **Publish**.

     ![binding](sb.png)

  2. Select **`OnlineShop`** in your service binding and click **Preview** to open SAP Fiori Elements preview.

     ![preview](sb2.png)


### Alternative On-premise - Publish service binding and run SAP Fiori Elements preview

  1. Login to **SAP NetWeaver** and execute `/n/IWFND/V4_ADMIN` `t-code`.

     ![preview](onprem.png)

  2. Click on **Publish Service Groups**.

     ![preview](onprem2.png)

  3. Select **System Alias** and click **Get Service Groups**. Select the Service Group **`ZUI_ONLINESHOP_O4_XXX`** and click **Publish Service Groups**.

     ![preview](onpremx.png)

  4. Click **Continue**, you will get the message **New service group(s) successfully published**.
    
     ![preview](onprem3.png)

  5. Click the back button to see the published group.

  6. Expand the **Service Groups** and double click on the group.

      ![preview](onpremisepublish.png)

  7. We can see the available services under this group.

  8. To test the service, go back to your service binding, select **`OnlineShop`** to start the SAP Fiori Elements preview.

      ![preview](sb2.png)


     More information [here](https://launchpad.support.sap.com/#/notes/2948977).

 

### Test yourself
