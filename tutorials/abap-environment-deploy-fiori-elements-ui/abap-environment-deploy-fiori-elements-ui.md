---
parser: v2
auto_validation: true
primary_tag: software-product>sap-btp--abap-environment
tags: [  tutorial>beginner, topic>abap-development, software-product>sap-business-technology-platform, software-product>sap-business-application-studio ]
time: 15
author_name: Merve Temel
author_profile: https://github.com/mervey45
---

# Create an SAP Fiori App and Deploy it to SAP BTP, ABAP Environment
<!-- description --> Create an SAP Fiori app for a RAP business object from SAP BTP, ABAP Environment in SAP Business Application Studio and deploy it to SAP BTP, ABAP Environment.

## Prerequisites  
- **Trial:** You need an SAP BTP, ABAP environment [trial user](abap-environment-trial-onboarding) or a license.
- **Licensed system:**
    - You have created the [Travel App Mission](mission.sap-fiori-abap-rap100). Mandatory is only the first tutorial of this tutorial mission [Create Database Table and Generate UI Service](abap-environment-rap100-generate-ui-service).
    - The business catalog `SAP_A4C_BC_DEV_UID_PC` (Development - UI Deployment) needs to be assigned to a business role of the developer user. For an existing ABAP systems, the business catalog needs to be added manually to the existing developer business role.
    - You need to be an organization manager at the used Cloud Foundry subaccount
    - You need to be a security administrator at the used Cloud Foundry Subaccount​
    - The SAP Business Application Studio and the SAP BTP, ABAP environment instance must be under same subaccount.
    - Read following blog post to get an overview on [Connecting from SAP Business Application Studio to SAP BTP Cloud Foundry environment](https://blogs.sap.com/2021/04/21/connecting-from-sap-business-application-studio-to-sap-btp-cloud-foundry-environment/)

## You will learn  
- How to assign role collections
- How to create dev spaces
- How to set up organization and space
- How to create list report object pages
- How to run SAP Fiori applications
- How to deploy applications
- How to check BSP library in Eclipse
- How to create IAM apps and business catalogs


---
### Assign role collection to user

  1. Login to [SAP BTP Trial cockpit](https://cockpit.hanatrial.ondemand.com/) and click **Enter Your Trial Account**.

      ![assign role collection](bas1.png)

  2. Select your subaccount **trial**.

      ![assign role collection](bas2.png)

  3. Now you are in the trial overview page. Click **Users** and **>**.

      ![assign role collection](btp.png)

  4. Select the menu and click **Assign Role Collection**.

      ![assign role collection](btp2.png)

      **Hint:** If you are using a licensed system, make sure you have the trust administrator role assigned to your user.

  5. Select **`Business_Application_Studio_Developer`** and click **Assign Role Collection**.

      ![assign role collection](btp3.png)

  6. Check your result. Now your user should have the **`Business_Application_Studio_Developer`** role collection assigned.

      ![assign role collection](btp4.png)



### Create dev space


  1. Select **trial**.

      ![dev](trial.png)

  2. Select **Service Marketplace** and search for **SAP Business Application Studio**. Select actions and click **Go to Application**.

      ![dev](studio212.png)

  3. Check the privacy statement and click **OK**.

      ![dev](studio2.png)

  4. Now the SAP Business Application Studio has started. Click **Create Dev Space**.

      ![dev](studio3.png)

  5. Create a new dev space:
       - Name: **Fiori**
       - Type: **SAP Fiori**

       Click **Create Dev Space**.

     ![dev](studio4.png)

  6. When your status is **Running**, select your dev space **Fiori**.

      ![dev](studio5.png)

### Set project folder

  1. Now you are in your **Fiori** dev space in SAP Business Application Studio.
     Select the menu on the left side and click **Open Folder** to set your workspace.

      ![organization](basn.png)

  2. Select **`/home/user/projects/`** and click **OK**.

      ![organization](basn2.png)

### Set up organization and space


  1. Switch to SAP BTP Trial and select your trial subaccount.

      ![organization](api.png)

  2. Copy your **Cloud Foundry Environment API endpoint** for later use.      

      ![organization](api2.png)

  3. Switch to **SAP Business Application Studio**, select the menu on the left side and click **View > Command Palette**.

      ![organization](s8.png)

  4. Search for **CF: Login to Cloud Foundry** and select it.

      ![organization](s9.png)

  5. Paste your Cloud Foundry API endpoint, enter your credentials and click **Sign in**.

      ![organization](s10.png)

  6. Now you can see, that you are logged in. Set now your cloud foundry target:

     - Cloud Foundry Organization: `<your_global_account>`
     - Cloud Foundry Space: dev

      ![organization](s11.png)

      Click **Apply**.



### Create list report object page

 1. Select the menu on the left side and click **View > Command Palette**. 

      ![object](basn7.png)

 2. Search for **Fiori: Open Application Generator** and select it.

      ![object](basn8.png)

 3. Select **List Report Page** and click **Next >**.

      ![object](basn9.png)

  4. Configure data source, system and service:
     - Data source: **Connect to a System**
     - System: **`<your_abap_trial_system>`**
     - Service: **`ZRAP100_UI_TRAVEL_O4_### (0001) - OData V4`**

     ![object](page.png)

     Click **Next >**.

  5. Select your main entity **`Travel`**, check **Yes** for adding automatically table columns and click **Next >**.

     ![object](page2.png)

  6. Configure project attributes:  
     - Name: **`ZRAP100_###`**
     - Title: **Travel App ###**
     - Description: **A Fiori application.**
     - Add deployment configuration: Yes  
     - Add FLP configuration: Yes 
     - Configure advanced options: No

     Click **Next >**.

     ![object](page3.png)

    **Hint:** Your **application name must** begin with a `z letter` and **must** be in **lowercase letters**.

  7. Configure deployment:

       - Target: ABAP
       - Destination: `<your_abap_trial_system>`
       - SAP UI5 ABAP Repository: `ZRAP100_###`
       - Package: `ZRAP100_###`
       - Transport Request: `<your_transport_request>`

     ![app](page4.png)

       Click **Next >**.

     **Hint:** If you want to copy your transport request, please do following:  Open Eclipse, search your package **`ZRAP100_###`** and open it. Open your transport organizer to see your transport request. The transport request of the superior folder needs to be chosen. Copy your transport request for later use. You can find your **transport request** underneath the **Modifiable** folder.
      ![app](transportrequest.png)

  8. Configure Fiori Launchpad:

       - Semantic Object: `ZRAP100_###`
       - Action: display
       - Title: Travel App ###

      ![app](page5.png)

      Click **Finish**.

  9. Now all files have been generated.



### Run SAP Fiori application for data preview


  1. On the **Application Information** overview page select **Preview Application**.

      ![run](page6.png)

     >**Hint.** You can open the **Application Information** through the menu > **View** > **Command Palette** > search and select **Application Information**.

  2. Select the first entry.

      ![run](page7.png)

  3. You browser opens. Click **Go** and check your result.

      ![run](page27.png)


### Deploy your application


1. On the **Application Information** overview page select  **Deploy Application**.

    ![deploy](page8.png)

2. When prompted, check deployment configuration and press y. 
    ![deploy](page9.png)

3.  When the deployment is successful, you will get these two information back as a result: **UIAD details** and **deployment successful**.

    ![deploy](page10.png)


### Check BSP library and SAP Fiori Launchpad app descriptor item in Eclipse


  1. Open Eclipse and check the **BSP library** and **SAP Fiori Launchpad app descriptor item folder** in your package **`ZRAP100_###`**. If you are not able to see BSP applications and SAP Fiori Launchpad app description items, refresh your package `ZRAP100_###` by pressing `F5`.

     ![library](page11.png)


### Create IAM App 


  1. In Eclipse right-click your package **`ZRAP100_###`** and select **New** > **Other Repository Object**.

      ![iam](page12.png)

  2. Search for **IAM App**, select it and click **Next >**.

      ![iam](iam.png)

  3. Create a new IAM App:
     - Name: **`ZTRAVEL_IAM_###`**
     - Description: IAM App for travel app

      ![iam](page13.png)

      Click **Next >**.

  4. Click **Finish**.

      ![iam](page14.png)

  5. Select **Services** and add a new one.

      ![iam](page15.png)

  6. Select following:
      - Service Type: `OData V4`
      - Service Name: `ZRAP100_UI_TRAVEL_O4_###`  

      ![iam](page16.png)

      **Hint:** With **CTRL + Space** you can search for your Service.

      Click **OK**.

      **Save** and **activate** your IAM app.

      ![iam](page17.png)


### Create business catalog

  1. Right-click your package **`ZRAP100_###`** and select  **New** > **Other Repository Object**.

      ![catalog](page12.png)

  2. Search for **Business Catalog**, select it and click **Next >**.

      ![catalog](catalog.png)

  3. Create a new business catalog:
     - Name: **`ZTRAVEL_BC_###`**
     - Description: Business catalog for travel app
 
      ![catalog](page18.png)

      Click **Next >**.

  4. Click **Finish**.

      ![catalog](page19.png)

  5. Select **Apps** and add a new one.

      ![catalog](page20.png)

  6. Create a new business catalog:
     - IAM App: `ZTRAVEL_IAM_###_EXT` 
     - Name: `ZTRAVEL_BC_###_0001`
     - Description: Business Catalog to IAM App assignment

      ![catalog](page21.png)

      Click **Next >**.

  7. Click **Finish**.

       ![catalog](page22.png)

  8. Click **Publish Locally** to publish your business catalog.

       ![catalog](page23.png)




### Run SAP Fiori application

  1. On the **Application Information** overview page select  **Deploy Application**.

     ![deploy](page24.png)

  2. When prompted, check deployment configuration and press y. 
 
     ![deploy](page10.png)

  3. Press **`CTRL and click the following link`** to open the URL in a browser.

     ![deploy](basn20.png)

  4. Log in to your system.

      ![url](login.png)

  5. Click **Go** and check your result.

      ![url](basn21.png)




### Test yourself
