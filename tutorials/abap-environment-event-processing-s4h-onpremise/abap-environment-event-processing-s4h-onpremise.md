---
parser: v2
auto_validation: true
time: 45
tags: [ tutorial>beginner, tutorial>license]
primary_tag: programming-tool>abap-development
author_name: Niloofar Flothkoetter
author_profile: https://github.com/niloofar-flothkoetter
---

# Event Processing in an SAP S/4HANA on-premise system
<!-- description --> Create and consume of business events in an On-Premise system

## Prerequisites
 - You need to have access to two On-Premise 2022 systems with authority to create and maintain a channel. 
 - You need to prepare an event mesh instance in your SAP business technology platform system and download the service key of this instance. For more information see the [Create Instance of SAP Event Mesh] (https://developers.sap.com/tutorials/cp-enterprisemessaging-instance-create.html)


## You will learn
  
  - How to set up a channel to connect to SAP Event Mesh 
  - How to consume an Event with RAP in an On-Premise system

## Intro
>Always replace `####` with your initials or group number.

The ABAP RESTful Application Programming Model (RAP) now supports the native consumption and exposure of business events from release 2022. For exposure, an event can be defined and raised in a RAP business object or in the behavior extension and then published via Event Bindings.


### Download event specification file 
An Event Consumption Model is a set of ABAP artifacts which allows you to receive events from external applications and to consume them within your business application. You can have a wide range of event producers. So, you need to know how the emitted event exactly looks like. To this aim, you need a file which contains the metadata of this event like how the payload looks like, what kind of types are there and so on. This is all specified in the event specification file, in this case, `AsyncAPI` specification, which is supported by SAP and can be downloaded from SAP API Business Hub in JSON format. For this aim, you need to download an event specification for the event **business partner** from the API hub.
    
1. Login to [SAP Business Hub] (https://api.sap.com/) and choose **SAP S/4HANA**. 

    ![API](1-1.png)

2. Under **SAP S/4HANA** choose **Events** tab and search for **business partner**.

    ![events](1-2.png)

3. Click **Event Specification** and download the JSON file.

    ![json](1-3.png)


### Create event consumption model in ADT

Here you will create an Event Consumption Model with the `.json` file that you downloaded in last step.

  1. Open ADT and open your S/4HANA system.

  2. Create a new **ABAP Package** if you have not one. Please be sure if your package name is with **Z** like

    - Name: `ZEVENT_CONSUMPTION_####`
    - Description: `event consumption`

    ![new](2-1.png)

  3. Right-click your package and choose **New** > **Other ABAP Repository Object** > **Business Services** > **Event Consumption Model**  and click **Next** to launch the creation wizard.

    ![new](4-1.png)

    ![new](2-3.png)

  4. Fill the fields and upload the `.json` file you saved before into the new event consumption ADT wizard. This will then automatically generate all that you need in this event consumption model, like the event handler custom code, authorization defaults values and inbound service.

    - Name: will be created with the Prefix and Identifier
    - Description: `event consumption model`
    - Namespace/ Prefix/ Identifier : `Z`and `EVENT####`
    - Event specification File: `.json` file

    click **Next**.

    ![event](2-4.png)

  5. Select all the event types that you would like to consume in your business application and click **Next**.

    ![event type](2-5.png)

  6. Click **Next** by **Define Consumer Artifacts**

    ![define](4-7.png)

  7. In **ABAP Artifact Generation List** click **Next**.

    ![generate](2-6.png)

   8. Select a **Transport Request** and click **Finish**.

    ![transport](4-9.png)

  9. In created event consumption model you can see the selected **Event Types** that are assigned because of your `.json` file.

    ![types](2-7.png)

  10. Save and activate your event consumption model.


### Create database table for handler method 

In this step, you can create a DB table and save the business partner IDs in this DB table. With this you can consume the event which is created in a S/4 system. Creating a DB table in this tutorial is just to help you to save and check your created IDs.

  1. Right-click your package and choose **New** > **Other ABAP Repository Object** > **Database Table** and click **Next**

    ![new](8-2.png)

    ![database](8-3.png)

  2. Enter the following name and description:

    - Name:`ZTABLE_####`
    - Description: `Database table`

    ![database](8-4.png)

    click **Next**, select a **Transport Request** and click **Finish** .

  3. Define the table with this code and replace all **####** with your number:

    ```ZTABLE_####
      @EndUserText.label : 'Database table'
      @AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
      @AbapCatalog.tableCategory : #TRANSPARENT
      @AbapCatalog.deliveryClass : #A
      @AbapCatalog.dataMaintenance : #RESTRICTED
      define table ztable_#### {
        key client     : abap.clnt not null;
        key recorddate : abap.dats not null;
        key recordtime : abap.tims not null;
        payload        : abap.char(1000);

      }
    ```

  4. Save and activate the table.


### Define handle event methods

  In the consumer extension class, the event processing logic can be implemented. In the generated class, you can find for each event type of the Event Consumption Model a respective handle event method. In each method, the corresponding event type and the typed business data for this event can be found. The latter is commented out so that you can implement your own logic.

  1. Navigate to **Business Service** > **Event Consumption models** > `ZEVENT####` > **Classes** > `ZCL_EVENT####` which is generated (or you can navigate to the **Source Code Library** > `ZCL_EVENT####`). Here you can see the create Method.

    ![class](8-0.png)

    ![class](3-1.png)


  2. Remove comment from these lines:

    ```
      DATA ls_business_data TYPE STRUCTURE FOR HIERARCHY Z_BusinessPartner_Created_v1.      
      ls_business_data = io_event->get_business_data( ).
    ```

  3. You need to copy the code below and replace it in your method. Replace all **####** with your number:

    ```ZCL_EVENT####
      DATA ls_business_data TYPE STRUCTURE FOR HIERARCHY Z_BusinessPartner_Created_v1.

      DATA wa TYPE  ztable_####.
      ls_business_data = io_event->get_business_data( ).

      wa-payload = ls_business_data-BusinessPartner.

      DATA(sydatum) = cl_abap_context_info=>get_system_date(  ).
      DATA(sytime) = cl_abap_context_info=>get_system_time(  ).

      wa-recorddate = sydatum.
      wa-recordtime = sytime.

      DELETE FROM  ztable_####
      WHERE recorddate < @sydatum.

      INSERT ztable_#### FROM @wa.

    ```  

  4. Your code will look like the following:

    ![class](3-2.png)

    >You can raise an exception whenever there is an issue. This will imply that the event processing has failed and therefore set the event status to **failed**. To see the exception, open **Window** > **Show View** > **Other** search for **Feed Reader**. Here you can see a message if an issue is raised.
    ![class](8-7.png)

### Channel Configuration 

In order to successfully exchange events between SAP Event Mesh and an SAP S/4HANA on-premise system, a corresponding connection is required. This connection is maintained by the Enterprise Event Enablement framework during channel creation. To consume events, the inbound bindings and subscriptions of the channel must be configured as well. 

### Creating a channel using a service key 

A channel represents a single connection to a service instance of the SAP Event Mesh. As a prerequisite, you need to prepare an Event Mesh instance in your SAP Business Technology Platform account and download the service key of this instance.  

  1. Open your On-Premise system in SAP Logon and run the transaction `/n/IWXBE/CONFIG`.

    ![run](5-1.png)

  2. Press **via Service Key** > **Default** to create a new channel.

    ![new](5-2.png)

  3. Enter the following name and description and replace all **####** with your number, enter the service key of your Event Mesh instance into the corresponding field and press save icon.

    - Name:`ZEVENT_CHANNEL_####`
    - Description: `New Channel ####`

    ![name](5-3.png)

  4. Your channel is created. Choose your channel and press **Activate**.

    ![new](5-4.png)


After creating a channel, you can decide which events should be listed on this channel. This explicit step of maintaining an outbound/inbound binding is necessary to publish/consume events from/in an S/4HANA system.  

### Configure inbound bindings 

  1. In the channel configuration UI click on **Inbound Bindings** to start configuring the event topics for the event consumption. Alternatively, you can run `/n/IWXBE/INBOUND_CFG`. 

    ![inbound](6-1.png)
    
  2. Choose your active channel and click on create new topic binding icon.

    ![new](6-2.png)

  3. Choose the search help to find the corresponding topic you wish to create an inbound binding for. in this case, choose `sap/s4/beh/businesspartner/v1/BusinessPartner/Created/v1` and switch to the next page.

    ![new](6-4.png)

    ![new](6-3.png)

    ![new](6-5.png)

  4. Choose your consumer, `ZEVENT####` and click **Create Destination**.

    ![dest](6-6.png)

  5. Enter your user and click save icon. Please keep in mind that the specified user here is the user that runs the consumer code in the event consumption model. So, one should make sure the specified user has all the necessary authorizations. 

    ![dest](6-7.png)
  
  6. In **Create Inbound Binding** screen you can see that a destination is created, now click save icon as well.

    ![dest](6-8.png)

 
### Configure subscriptions

  1. You need to create a queue for your Event Mesh instance. How to get access to the SAP Event Mesh and creating a queue is already explained in this [blog post] (https://blogs.sap.com/2022/08/19/an-introduction-to-enterprise-event-enablement-for-sap-btp-abap-environment/) in details.. For more information about how to create a queue see also this [link] (https://developers.sap.com/tutorials/cp-enterprisemessaging-queue-queuesubscription.html)

  2. The second step of the event consumption configuration is the creation of queue subscriptions for the channel. Back to your On-Premise system. In the channel configuration UI click **Subscriptions** to start configuring the queue subscriptions for the event consumption. Alternatively, you can run `/n/IWXBE/SUBSCRIPTION`. 

    ![subs](7-1.png)

  3. In the **Create Channel Subscription** choose your active channel, click create new subscription icon and enter the address of the queue you already created for your Event Mesh instance and click Save.

     ![queue](7-2.png)

     ![queue](7-3.png)

  Now you can consume an event using the event consumption model embedded in ABAP Development Tools for Eclipse (ADT). 

### Create business partner

To test your event consumption scenario, a business partner must be created in the event producer system (which can be any cloud or OP system). Based on the logic you defined in the handler event method, as soon as a new business partner is created, the event will be sent to the Event Mesh and you will see the ID of the created business partner in the table.  

If you want to send the event from an On-Premise system to the Event Mesh, you need to have an active channel and configure the outbound topic bindings for this channel. 

  1. Open your second On-Premise system in SAP Logon and create and activate a channel like in step 6. In this step you can use a name like `ZEVENT_TEST_####`

  2. To configure your channel run `/n/IWXBE/OUTBOUND_CFG` or click **Outbound Bindings**.

    ![conf](9-1.png)

  3. In the configuration UI select your channel and click **Create new topic binding** icon and Choose the search help to find the corresponding topic you wish to create an inbound binding for. in this case, choose `sap/s4/beh/businesspartner/v1/BusinessPartner/Created/v1` and click save icon.

    ![topic](9-2.png)

  4. Check your topic is listed and run the transaction **BP** to create a new business partner. 

    ![topic](9-3.png)

  5. In Business Partner UI click **Person** and enter a name and last name and click **Save**.

    ![person](9-4.png)

  6. A new business partner will be created and you can see the ID.

    ![person](9-5.png)

  7. Now open your first On-Premise system in ADT and open your database table `ZTABLE_####` and run the table with F8. You can find your created business ID in this table.

    ![table](9-6.png)

### Test yourself
