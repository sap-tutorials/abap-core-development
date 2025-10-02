---
parser: v2
keywords: tutorial
auto_validation: true
time: 45
tags: [tutorial>intermediate, programming-tool>abap-development, software-product>sap-business-technology-platform, tutorial>license]
primary_tag: software-product>sap-btp--abap-environment
author_name: Julie Plummer
author_profile: https://github.com/julieplummer20 

---

# Expose RAP Business Event in SAP BTP ABAP Environment

## Prerequisites
 - **Caution:** You cannot complete this tutorial with a trial version.
 - You have created a RAP Business Event, for example using this tutorial: [Create a RAP Business Event in SAP BTP ABAP Environment](https://developers.sap.com/tutorials/abap-environment-create-rap-business-events.html)
 - You have access to a SAP BTP ABAP environment system **with a license**. 
 - You have access to Event Mesh in SAP Integration Suite. [Getting Started with SAP Integration Suite](https://help.sap.com/docs/integration-suite/sap-integration-suite/initial-setup). 
 
     
    > **Note**: SAP Event Mesh Service is no longer available for new customers unless you already have access to it. In that case you can use it as a message broker and its instance to create the channel, otherwise you need to add Event Mesh service in SAP Integration Suite. If you are an existing customer of the standalone SAP Event Mesh service (default plan), you can not subscribe to the SAP Integration Suite Event Mesh capability in the same subaccount. In such cases, you must subscribe to SAP Integration Suite in a different subaccount.

    > If you don't have Integration Suite Event Mesh service then you first need to add Event Mesh capability to have access to it by clicking on 'Add Capabilities' in your Integration Suite cockpit. Once the capability is activated, configure user access by assigning the appropriate roles and role collections to the users who want to work with the capabilities. See [Configuring User Access to SAP Integration Suite](https://help.sap.com/docs/integration-suite/sap-integration-suite/configuring-user-access).
    
    >  Now you need to create the Event Mesh instance on your subaccount. The service instance SAP Integration Suite, Event Mesh on the SAP BTP cockpit provides access to the message client of Event Mesh capability. Configuring a message client is a two-step process. First, you create a service instance and then create a binding to the Event Mesh capability. See [Configure A Message Client](https://help.sap.com/docs/integration-suite/sap-integration-suite/configure-message-client). Then you need to prepare an Event Mesh instance in your SAP business technology platform system and have downloaded the service key of this instance. 

 - You need a user with access to maintain communication arrangement <!-- REVISE -->


### Create an Event Binding for the Business Event

  Now create the event binding for your newly created business event. This event binding is needed to map an event to a RAP entity event.

  1. Right-click on your package and create an event binding

    - Name: `ZEVENT_EXPOSURE_###`
    - Description: `RAP business event`

  <!-- border -->
  ![binding](7-1.png)

  <!-- border -->
  ![binding](7-2.png)

  3. Here fill all fields out, to get errors gone. You can freely choose these names to specify your event with some considerations explained below

    - Namespace: `zevent###` (No camel case and no space)
    - Business Object: `OnlineShop` (No Space)
    - Business Object Operation: `create`

   <!-- border -->  
   ![error](7-3.png)

  4. Click **Add** to add items.

    - Root Entity Name: `ZR_ONLINE_SHOP_###` (your behavior definition)
    - Entity Event Name: `ITEMISORDERED` (Event name in your behavior definition)

   <!-- border -->
  ![item](7-4.png)

  5. Save and activate your event binding.

  6. As you can see at the screenshot, **Type** (aka topic) is a concatenation of the three attributes (name space, business object, business object operation) and ends with the version of the event. The wildcard * points to the corresponding event e.g. created. This is relevant for addressing the events to the Event Mesh. Copy this address for later use `zevent###.OnlineShop.create.v*`.

   <!-- border -->
  ![type](7-5.png)


### Create a communication arrangement

  After an event is raised, it should arrive at the Event Mesh to be consumed later. The connection between the system and the SAP Integration Suite Event Mesh is achieved through a channel. For this, you need to create a communication arrangement in your cloud system with `sap_com_0092` scenario which is built for an event enablement and also an Event Mesh instance's service key.

  One requirement is that you have an existing SAP Integration Suite Event Mesh service instance. You can create an instance of SAP Integration Suite Event Mesh provided that you already subscribe to the Integration Suite Event Mesh capability.

>For more information about how to create instance of SAP Event Mesh [link] (https://developers.sap.com/tutorials/cp-enterprisemessaging-instance-create.html)

  1. Log on to your cloud system and navigate to **Communication Arrangement**.
 
  <!-- border -->
  ![comm](8-1.png)

  2. Click **New** to create a new communication arrangement.
  
  <!-- border -->
  ![new](8-2.png)
  
  <!-- border -->
  ![new](8-3.png)

  3. Choose `sap_com_0092` as **Scenario** and copy the service key of your Integration Suite Event Mesh instance under **Service Key**.

   <!-- border -->
   ![comm](8-4.png)

   <!-- border -->
   ![comm](8-5.png)

  4. Create a **Communication User**. Click **New** and enter a **User Name**, **Description** and **Propose Password**. Copy the generated password and save it for later. Click **Create**.

  <!-- border -->
  ![user](8-6.png)

 <!-- border -->
 ![user](8-7.png)

  5. Now change the **Arrangement Name** to `Z_EVT_0092_###` and replace `###` with your initials or group number. This Arrangement Name will also be the name of the channel which is used later to send events.

  - Click **Create** communication arrangement.

  <!-- border -->
  ![name](8-8.png)  

  6. Open your newly created communication arrangement and check the connection under **Outbound Services** to check if the connection is established and the channel is active.

   <!-- border -->
   ![details](8-9.png)
   
   <!-- border -->
   ![details](8-10.png)


### Assign the event to an outbound channel


  1. Search for **Enterprise Event Enablement** App and open it.
   <!-- border -->
   ![app](9-1.png)

  2. Click **Go** to open a list of channels and choose your channel in this list.
   <!-- border -->
   ![channel](9-2.png)

  3. Now add the outbound topic, which is generated during the event binding generation, in to this channel:

  - Click **Create**  
  <!-- border -->
  ![create](9-3.png)

  - On the next page click **Topic** value help
  <!-- border -->
  ![create](9-5.png)

  - In this popup search for `zevent###/OnlineShop/create/*` what you copied in step 8-5 and choose it. Replace `###` with your number.
  <!-- border -->   
  ![create](9-4.png)

  - Click **Create**
  <!-- border --> 
  ![create](9-6.png)
 
  <!-- border -->
  ![create](9-7.png)

  4. In your channel click on **Metadata**, download the AsyncAPI json file, open it to copy the channels data for later use.

  <!-- border -->
  ![metadata](9-8.png)

 
  5. Open your cloud system cockpit and navigate to **Instances and Subscriptions** and open **Integration Suite** application. Then click on Manage Business Events- Configure Events

   <!-- border -->
   ![event](9-9.png)
    
   
  6. Open the message client, navigate to **Queues** and click **Create Queue**.

   <!-- border -->
   ![queue](9-10.png)

   <!-- border -->
   ![queue](9-11.png)

  7. Enter a queue name and click **Create**.

  - Queue Name: `onlineshop/###` (replace `###` with your number)

   <!-- border -->
   ![name](9-12.png)

  8. Double click on your newly created queue and on tab **Subscriptions** click on **Create**. Here enter the topic what you copied from the event metadata and click **Create**. The subscription to the selected topic is done. 

   <!-- border -->
   ![subs](9-13.png)

   <!-- border -->
   ![subs](9-14.png)

   


### Create a Message in Application


  1. Open ADT and open your SAP BTP ABAP environment system.

  2. Navigate to your service binding `ZUI_Z_ONLINE_SHOP_###_O4` and click **Preview** to open your application.
   
   <!-- border -->
   ![preview](11-1.png)

  3. Click **Create** to order a new item.

   <!-- border -->
   ![order](11-2.png)

   <!-- border -->
   ![order](11-3.png)

   <!-- border -->
   ![order](11-4.png)

  4. Back to your cockpit and check in your event mesh under **Queues** if this event has reached the system. If you check the queue, a new message is arrived at the queue.

   <!-- border -->
   ![message](11-5.png)

  5. Navigate to **Test** and choose your queue  under **Queue** in **Consume Messages** section.


  6. Click **Consume** to consume this event in your applications. Here you can see that you have the type which you have find in the event binding and the transported data is the parameter structure.
 
   <!-- border -->
   ![message](11-6.png)

  
  You can also consume this event using [Event Consumption Model within a Business Application](https://developers.sap.com/tutorials/abap-environment-event-enablement-consumption.html#top)
  
    

### Test yourself


## More information
