---
title: Build an Integral SAP Integration Suite project and Consume it from a SAP AppGyver Custom App
description: It is intentioned for consultants/users who want to exercise their integration skills using SAP Integration suite capabilities and SAP AppGyver to quickly develop a custom app that can consume SAP Integration Suite APIs.
auto_validation: true
time: 10
tags: [ tutorial>beginner, software-product>sap-integration-suite, software-product>api-management, software-product>sap-appgyver, software-product>sap-business-technology-platform, tutorial>free-tier ]
primary_tag: software-product>sap-integration-suite
parser: v2
author_name: Mariajose Martinez
author_profile: https://github.com/mariajosesap
---
# Build an Integral SAP Integration Suite project and Consume it from a SAP AppGyver Custom App
<!-- description --> It is intentioned for consultants/users who want to exercise their integration skills using SAP Integration suite capabilities and SAP AppGyver to quickly develop a custom app that can consume SAP Integration Suite APIs.

## Prerequisites

 - You have a SAP BTP account or trial account. If you don’t have an account, it is recommended to start using the several SAP BTP Free-Tier services available, this [tutorial](btp-free-tier-account) can help you. If you want to use the “regular” trial account, create one by registering [here](https://account.hana.ondemand.com/).
 

## You will learn

  - How to consume a SAP Open Connector service to connect to a 3rd party application. In this case Stripe, for a payment transaction
  - How to generate a Sales Order consuming an OData service from a SAP Sales and Service Core (formerly SAP Cloud for Customer or C4C) environment
  - How to set up SMS as notification after the sales order is successfully created, using a Twilio API
  - How to apply API Management Policies to SAP Cloud Integration to avoid CORS issues and to securely trigger calls to the API with an API Key without having to expose the SAP BTP / Integration Suite credentials
  - How to integrate it with SAP AppGyver, by enabling in the Data Connection the API Key to call the Cloud Integration Flow from SAP AppGyver

## Intro

To illustrate, here’s the demo scenario’s architecture:

![Demo Scenario's Architecture](scenario.png)

>For authentication purposes, I’m using the SAP default Identity Provider which is already configured in the SAP BTP trial account.


### Get started with setup prerequisites

1.  Create a SAP Integration Suite instance in the “Instances and Subscriptions” tab:
    
    ![Instance](prerequisite.png)

2.  Click on the already generated SAP Integration Suite instance and add the capabilities: Cloud Integration, API Management (with its API Business Enterprise Hub) and Open Connectors. In my case, I added all to have full capabilities, but you don’t need them all for this scenario.
    
    ![Integration Suite Provisioning](is_provisioning.png)

3.  Go back to the `Security-->Users` tab and make sure you add the `MessageSend` role, because you’ll need it to execute requests to the Integration Flow 
    >I had to add it manually in by Assigning its Role Collection.

    ![MessageSend Role](security.png)

4.  Go back to the `Trial Home--> <BTP Account Id>` and click on **`Boosters`** to start the Integration Suite Booster (this task will create the instance, roles and services keys needed to use the service).

    ![Boosters](Boosters.png)

5.  You have a Stripe account or trial account. If you don’t have one, you can create one [here](https://stripe.com/docs/development). When done, go to “Dashboards” and create a new customer information. Add a testing card to this new customer, you can select one here. I created a customer called “Jane Doe”.

    ![Stripe Account](stripe.png)

6.   You have a SAP Sales and Service Core tenant (formerly SAP Cloud for Customer or SAP C4C) with admin rights. Unfortunately a trial isn’t available. But if you don’t have an account you may want to test this Sales Order creation exercise with any other platform, such as your SAP S/4HANA Cloud directly (if On Premise, follow this [blog post](https://blogs.sap.com/2022/05/09/create-purchase-orders-in-s-4hana-by-enabling-a-public-api-from-a-s-4hana-on-premise-system-using-sap-api-management-and-cloud-connector/) to know how to expose your API). If you don’t have a  S/4HANA environment, maybe you want to try out the free trial [here](https://www.sap.com/products/s4hana-erp/trial.html). (I haven’t test the integration with the S/4HANA Cloud trial environment, but you can try if you want to).

     - In SAP C4C, you have created the product with which we will simulate the purchase. This is the one I’ve created, a Samsung Refrigerator:
     
     ![Create Product](c4c.png)
     - You have a Twilio developers account or trial account. You can create your trial account here.
  
          - Configure a Twilio phone number (as the sender’s phone number)
          - Validate your phone number (to be able to test reception of SMS messages from Twilio)
          - And copy and paste your Twilio developer account Id, API’s user and password, we are going to use this later.
      ![Twilio](twilio.png)

     - You have an AppGyver account or trial account and you have created an application
          - You can create your trial account here.
          - Create a custom app, you can follow this blog post I created before.
  


### Consume a Stripe service from SAP Open Connectors and SAP Cloud Integration to create payment transactions 


### Set up Write, Filter and Get tasks in SAP Cloud Integration to save, filter and get your needed message to execute other operations in your Integration Flow 


### Consume a SAP Sales and Service Core API to create Sales Orders using an OData receiver adapter in SAP Cloud Integration 


### Send application/x-www-form-urlencoded data to a HTTP receiver adapter in SAP Cloud Integration to send SMS messages consuming a Twilio API 


### Integrate SAP AppGyver with SAP Integration Suite, consuming an Integration Flow levering SAP API Management policies 



