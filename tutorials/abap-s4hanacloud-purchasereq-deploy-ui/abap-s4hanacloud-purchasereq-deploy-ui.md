---
parser: v2
auto_validation: true
primary_tag: software-product-function>sap-s-4hana-cloud--abap-environment
tags:  [ tutorial>beginner, software-product-function>sap-s-4hana-cloud-abap-environment, programming-tool>abap-development, programming-tool>abap-extensibility]
time: 25
author_name: Merve Temel
author_profile: https://github.com/mervey45
--- 
 
# Create a SAP Fiori Online Shop App and Deploy it
<!-- description --> Create a SAP Fiori app for a business object in Visual Studio Code and deploy it.

## Prerequisites  
- This tutorial can be used in both SAP S/4HANA Cloud, private edition system and SAP S/4HANA on-premise system with release 2022 FPS01. We suggest using a [Fully-Activated Appliance](https://blogs.sap.com/2018/12/12/sap-s4hana-fully-activated-appliance-create-your-sap-s4hana-1809-system-in-a-fraction-of-the-usual-setup-time/) in SAP Cloud Appliance Library for an easy start without the need for system setup.
- The steps for fully-qualified Domain Name & SSL Certificates can be found [here](https://www.sap.com/documents/2020/06/109b1be0-9e7d-0010-87a3-c30de2ffd8ff.html).
- For SAP S/4HANA on-premise, create developer user with full development authorization 
- You have installed the latest [Eclipse with ADT](abap-install-adt).
- You set up [VS Code for Fiori Tools](https://developers.sap.com/tutorials/fiori-tools-vscode-setup.html)

## You will learn  
- How to create an ABAP package 
- How to create list report object pages
- How to run SAP Fiori applications
- How to deploy applications
- How to check BSP library in Eclipse


## Intro
In this tutorial, wherever XXX appears, use a number (e.g. 000).

This scenario describes the embedded deployment, which features an SAP Fiori front-end server (and the associated components) deployed along with the backend components in the same system.

---
 
### Create an ABAP package

In this step, we'll create an ABAP package with the **superpackage `$TMP`**. 

  1.  Select **Local Objects (`$TMP`)** > **New** > **ABAP Package**.

      ![package](package0.png)

  2.  Create new **ABAP package**:
       - Name: **`$Z_PURCHASE_REQ_TIER3_XXX`**
       - Description: Package XXX - Tier 3
       - `Superpackage:` **`$TMP`**
       - Package Type: Development
 
      ![package](package.png)

       Click **Next >**.

  3. Click **Next >**.

      ![package](package2.png)


  4. Click **Finish**.
  
      ![package](package3.png) 

 
  5. Now continue with step 2 of [Create an Online Shop Business Object](abap-s4hanacloud-procurement-purchasereq-shop). Move on with [Enhance the Behavior Definition and Behavior Implementation of the Online Shop Business Object](abap-s4hanacloud-purchasereq-enhance-shop) and [Integrate released purchase requisition API into Online Shop Business Object](abap-s4hanacloud-purchasereq-integrate-api). Afterwards, come back and continue with step 2 of this tutorial.
 

### Create list report object page

This step is for creating a list report object page. Open Visual Studio Code and open your visual studio projects folder

  1. Open **Visual Studio Code** and open your **Visual Studio Code projects folder**. Select the **projects** folder. Your created projects will later appear in this folder.

     ![object](vs.png)

    >**Hint:** You can find your folder under: `C:\Users\<userid>\projects`.

  2. Select the menu on the top and click **View > Command Palette**.

      ![object](vs2.png)

  3. Search for **Fiori: Open Application Generator** and select it.

      ![object](vs3.png)
 
  4. Select **Template Type: SAP Fiori**, as template **List Report Page** and click **Next >**.

      ![object](vs4.png)

  5. Configure data source, system and service:
     - Data source: **Connect to a System**
     - System: New System
     - System type: **ABAP On Premise**
     - System URL: `https://mysystem.fqdn.com:44301`
     - SAP client: 100
     - Service username: `<your_username>`
     - Service password: `<your_password>`
       
       Click the login symbol on the right side to log in.

     - System name: `<your_system_ur>`
     - Service: **`ZUI_ONLINESHOP_O4_XXX > ZUI_ONLINESHOP_XXX(0001) - OData V4`**

     ![object](vs5.png)

     Click **Next >**.

    >**Hint:** In case of invalid security certificate errors, follow this [link](https://help.sap.com/docs/SAP_FIORI_tools/17d50220bcd848aa854c9c182d65b699/4b318bede7eb4021a8be385c46c74045.html).

  6. Select your main entity **`OnlineShop`** and click **Next >**.

      ![object](vs6.png)

  7. Configure project attributes:  
     - Name: **`z_purchase_req_tier3_xxx`**
     - Title: **Online Shop XXX**
     - Description: **A Fiori application.**
     - Project folder path: `c:\Users\<userid>\projects`
     - Add deployment configuration: Yes
     - Add FLP configuration: No
     - Configure advanced options: No

     Click **Next >**.

      ![object](vs7.png)

     An FLP configuration does not have to be created in Visual Studio Code, as this will be created in a later step using [tools for setting up the Launchpad content](https://help.sap.com/docs/FLP/6583b46f6c164aad818a3891bc91d8d8/08683e5409b74ced8705b2856c96c63b.html?state=DRAFT).

     The project folder path has a further substructure.

    >**Hint:** Your **module name must** be written in **lowercase letters**.

  8. Configure deployment:
       - Target: ABAP
       - Target system: `<your_abap_system_url>`
       - Enter client: 100
       - SAPUI5 ABAP Repository: `zshop_tier3_xxx`
       - Package: `$z_purchase_req_tier3_xxx`
       - How do you want to enter Transport Request? Manually
       - Transport Request: is not needed
  
      ![app](vs8.png)

     Click **Finish**.


### Run SAP Fiori application for data preview

In this step, we'll run the SAP Fiori application, to check 


  1. In the **Application Information** overview, select **Preview Application** to start the preview.

      ![deploy](vs9.png)

  2. Log in with your credentials. Then select the first entry in the search bar.
  
      ![url](vs10.png)

  3. Your browser opens. Click **Go** and check your application. 

      ![url](vs11.png)


### Deploy your application 

This step describes the deployment of the created application.


  1. In the **Application Information** overview, select **Deploy Application** to start the deployment.

      ![deploy](vs12.png)   

  2. When prompted, check deployment configuration and press y. Open the URL at the end of the deployment log in browser to preview the application.

      ![deploy](vs13.png)

      When the deployment is successful, you will get these two information back as a result: **deployment successful**.

    >**Hint:** If an error message appears, use the [Environment Check](https://help.sap.com/docs/SAP_FIORI_tools/17d50220bcd848aa854c9c182d65b699/75390cf5d81e43aea5db231ef4225268.html).


### Run your application standalone

  1. Open the URL to open the deployed application.

      ![deploy](vs14.png) 

  2. Click **Go** and check your application.

      ![deploy](vs11.png) 

### Check BSP library and SAP Fiori Launchpad app descriptor item in Eclipse


  1. Open Eclipse and check the **BSP library** in your package **`$z_purchase_req_tier3_xxx`**. You can find your package under **Local Objects (`$TMP`)** > **User123**. If you are not able to see BSP applications and SAP Fiori Launchpad app description items, refresh your package by pressing `F5`.

      ![library](bsp.png)


### Test yourself
