---
parser: v2
author_name: Achim Seubert
author_profile: https://github.com/achimseubert
auto_validation: true
time: 30
tags: [ tutorial>beginner, programming-tool>abap-development, products>sap-business-technology-platform]
primary_tag: products>sap-btp--abap-environment
---

# SAP BTP, ABAP system as Content Provider for SAP Launchpad Service
<!-- description --> Target of this tutorial is to provide content of a SAP BTP, ABAP environment system to the SAP Launchpad Service of a subaccount on SAP BTP.

## Prerequisites
 - Subscribe to the SAP Launchpad Service in the subaccount the content shall be consumed. Therefore follow the steps of tutorial
 ["Set Up the SAP Launchpad Service"](cp-portal-cloud-foundry-getting-started) in your subaccount.
 - **Authorization:** Your SAP BTP, ABAP environment user needs a business role with business catalogs **Administrator** (ID: `SAP_BR_ADMINISTRATOR role`) assigned.
 - **Authorization:** In the subaccount where the destinations for Content Federation will be created, the user needs to be a subaccount administrator with Role Collection `Launchpad_Admin` assigned.

## You will learn
  - How to setup a communication arrangement
  - How to expose roles to the SAP Launchpad Service
  - How to create the necessary destinations
  - How to create a Content Provider
  - How to create a Launchpad Site
---

### Create the Communication System


- Open the `Communication Systems` app from the SAP Fiori Launchpad in your SAP BTP, ABAP environment system.

- Create a new Communication system.

- Specify `System ID` and `System Name` for the new Communication System.

- In the field `Host Name` enter the callback URL. This is needed for setting up content change notifications. For example portal-service.cfapps.eu10.hana.ondemand.com.

  <!-- border -->![Communication system overview](1-communication-system.png)

- Add a technical user for inbound communication.

  <!-- border -->![New Inbound User](2-Users-for-inbound-communication.png)

  <!-- border -->![New Inbound User](3-New-Inbound-Communication-User.png)

- Authentication method is `User Name and Password`. Let the System generate a Password.
Do not forget to note the password down. It will be used in `Step 5` (Create Design-Time destination).

<!-- border -->![Inbound User settings](4-Propose-pwd-3steps.png)

- Similarly add a technical user for outbound communication with `Authentication Method` `None`.

  <!-- border -->![Outbound User settings](5-outbound-user.png)


### Create the Communication Arrangement


- Open the `Communication Arrangements` app from the SAP Fiori Launchpad and select `New`.

- In the `New Communication Arrangement` app choose Communication Scenario `SAP_COM_0647` for creation.

- Press `Create`.

- In the "Common Data" section set the Communication System created before using the value help.

<!-- border -->![Communication arrangement](7-communication-arrangement-settings.png)

- Enter the job execution details for scheduling the FLP Content Exposure job every minute and press `Save`.


### Select Roles for exposure to Launchpad Service


- Open the `Maintain Business Roles` app.

- Search for Business Role ID `SAP_BR_ADMINISTRATOR`.

- Mark Business Role `SAP_BR_ADMINISTRATOR`.

- Click `Expose to SAP Launchpad Service`.

<!-- border -->![Expose to the SAP Launchpad Service](8-expose-to-launchpad-service.png)

> The content related to the Business Role, such as groups, catalogs, pages or spaces can then be consumed by the SAP Launchpad Service.


### Configure communication between the SAP BTP subaccount and the SAP BTP, ABAP environment system


    - In the SAP BTP Cockpit download the trust certificate of the subaccount runtime destinations, by navigating to Connectivity -> Destinations. Press the `Download Trust` button.

    - Open the `Communication Systems` app from the Fiori Launchpad in your SAP BTP, ABAP environment system.

    - Create a new Communication system.

    - Specifiy the System ID and System Name for the new System.

    - Select `Inbound only`.

    - Set `SAML Bearer Assertion Provider` to `ON`.

    - Upload the certificate file that you downloaded from your subaccount as the Signing Certificate.

    - Specify a unique Provider Name in the following format:
      `cfapps.<**region**>.hana.ondemand.com/<unique_name>`

    - Save the communication system.



### Create the Design-Time Destination

We will create the design-time destination in order to define the location from which the SAP Launchpad service should fetch the exposed content.

- Go to the subaccount in which you want to use the Launchpad Service.

- Choose `Destinations` in the `Connectivity` menu item.

- Create a new Destination.

<!-- border -->![Settings for new Destination](1-Destination-settings.png)

**URL**:

- Use Service URL of section `Inbound Services` in the Communication Arrangement created in `Step 2` (Create the Communication Arrangement).

- Add suffix `/entities`.

- Format: `https://<guid>.abap.<region>.hana.ondemand.com/sap/bc/http/sap/aps_flp_content_exposure/entities`

**Authentication**: `BasicAuthentication`.

**User**: Inbound user with its password created in `Step 1` (Create the Communication System).

Click "Save".


### Create Runtime Destination for launching apps in an iFrame

Create a new Destination.

<!-- border -->![Settings for Runtime iFrame integration](3-Destination-iFrame.png)

**URL**: Use the domain of the Service URL in section "Inbound Services" in the Communication Arrangement created in `Step 2` (Create the Communication Arrangement). Replace `abap` with `abap-web`. (Format: https://<`tenant`>.**abap-web**.<`region`>.hana.ondemand.com)

**Additional Properties**:

Click on `New Property` to add new items.

**`HTML5.DynamicDestination`**: `true`

**`sap-platform`**: `ABAP`

Click "Save".


### Create Runtime Destination for dynamic OData access
 <br>
Create a new Destination.

<!-- border -->![Settings for dynamic Runtime Destination](2-Destination-oData.png)

**URL**: Use the domain of the  Service URL in section "Inbound Services" in the Communication Arrangement created in `Step 2` (Create the Communication Arrangement).
Format: https://<`tenant`>.**`abap`**.<`region`>.hana.ondemand.com

**`AuthnContextClassRef`**: `urn:oasis:names:tc:SAML:2.0:ac:classes:PreviousSession`

**Additional Properties**:

**`HTML5.DynamicDestination`**: `true`

**`nameIdFormat`**: `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`

Click "Save".




### Create Content Provider


- From the subaccount, navigate to `Services`,  `Instances and Subscriptions` and search for the `Launchpad Service` in the Search box

- Access Launchpad Service.

- Go to Provider Manager.

- Create a new Content Provider.

    <!-- border -->![Workflow](3-cp-overview.png)

- Add Title, Description, ID and the Destinations created in `Step 5-7` (Create Design-Time / Runtime destination) accordingly.

    <!-- border -->![Create Content Provider](1-create-content-provider.png)

- Click "Save".

> In the `Status` column of the table the status will be set to `Created` as soon as the creation is finished.


### Choose content in the Content Manager


    - Navigate to the Content Manager.

    - Open Tab `Content Explorer`. Here all the created Content Providers are listed.

    - Click on the Content Provider created in the previous Step.

    - Mark the item with Title `Administrator` and click `Add to My Content`.

    <!-- border -->![Add roles to own content](4-add-to-my-content.png)


### Create Launchpad Site


    - Navigate to the Site Directory.

    - Click `Create Sites`.

    - Choose a site name and click `Create`.

    - Click `Edit` and click into the box to `Search for items to assign`.


    <!-- border -->![Create Launchpad Site](2-create-site.png)

    - Choose the `Administrator` role (click on `+`).

    - Click `Save`.

    - Copy and store the URL of the Launchpad for later use.

    <!-- border -->![Launchpad URL](9-copy-launchpad-url.png)

> Now you assigned the Business Role, which you exposed to the Launchpad Service, to your Launchpad Site. With it all apps that are related to this Business Role will be available in the Launchpad Site. Also a Role Collection is created, which is named in this format: `<Content Provider name>_<Business Role name>`


### Add Role Collection to user


> In order to be able to access the Launchpad Site you have to assign the Role Collection created in the last Step, to the SAP BTP, ABAP environment user in the SAP BTP Cockpit.

- Open the BTP Cockpit.

- Go to the subaccount, of the Launchpad Service.

- Navigate to the `Users` menu.

- Search for and mark the BTP user of the SAP BTP, ABAP environment.

- Click on `Assign Role Collection` in the `Role Collection` section (might be hidden behind the `...`, dependent on the window size).

- Assign the created authorization to the user.

<!-- border -->![Assign created role collection to SAP BTP, ABAP environment user](1-add-role collection.png)


### Access Launchpad Site


Now access the Launchpad Site via the URL stored in `Step 10` with the SAP BTP user you assigned the Role Collection to in `Step 11`.



---
