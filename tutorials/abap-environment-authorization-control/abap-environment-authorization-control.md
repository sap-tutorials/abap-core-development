---
parser: v2
auto_validation: true
primary_tag: software-product>sap-btp--abap-environment
tags: [  tutorial>beginner, programming-tool>abap-development, software-product>sap-business-technology-platform, software-product>sap-s-4hana-cloud ]
time: 30
author_name: Patrick Winkler
author_profile: https://github.com/sepp4me
---

# Providing Authorization Control for a Business Configuration Maintenance Object
<!-- description --> Providing Authorization Control for a Business Configuration Maintenance Object

## Prerequisites  
- You need an SAP BTP, ABAP environment license. If you have only a trial account, you can skip this tutorial.
- This tutorial also works in an SAP S/4HANA Cloud, public edition system.
- This is the second tutorial of group [Create a SAP Fiori based Table Maintenance app](group.abap-env-factory). You must complete the tutorials in the specified order.


## You will learn  
- How to create an IAM app
- How to create Business Catalog
- How to create and assign an IAM Business Catalog to a Business Role

## Intro
[Authorization control in RAP](https://help.sap.com/viewer/923180ddb98240829d935862025004d6/Cloud/en-US/375a8124b22948688ac1c55297868d06.html) protects your business object from unauthorized access to data:

 - To protect data from unauthorized read access, ABAP CDS provides its own authorization concept based on a data control language (DCL).
 - Modify operations such as standard operations and actions can be checked against unauthorized access during RAP runtime.

For this purposes, the generated business object checks the authorization object `S_TABU_NAM` with the CDS entity `ZI_ERRORCODE###` and the activity `03` (read) / `02` (modify).

[To consume the service](https://help.sap.com/docs/btp/sap-abap-development-user-guide/consuming-services-in-ui) of the generated business object in the CUBCO app, you must define an IAM app and assign the service to the app. This ensures that you can define the required authorizations.

First, you create the IAM app yourself. As a next step, you create a business catalog and a business role that you can assign to your business user.

---
### Create IAM app


  1. Right-click the package **`Z_ERROR_CODES_###`** and choose **New** > **Other ABAP Repository Object**.

      ![New repository object](e.png)

  2. Search for **IAM App**, select it and click **Next >**.

      ![New IAM app](iam.png)

  3. Create new IAM app:
      - Name: **`Z_ERROR_CODES_###`**
      - Description: **`Error Codes - Maintenance`**
      - Application Type: **`MBC - Business Configuration App`**

     ![Enter IAM app definition](iam2.png)

      Click **Next >**.

  4. Select a Transport Request and click **Finish**.

  5. Choose **Services** and add a new service.

      ![Add service to IAM app](iam4.png)

  6. Select your service:
      - Service Type: **`OData V4`**
      - Service Name: **`ZUI_ERRORCODE000_O4`**

     ![Select service](iam5.png)

      Click **OK**.

  7. Choose **Authorizations** and add a new authorization object.

      ![Add authorization object](iam6.png)

  8. Search for **`S_TABU_NAM`** and click **OK**.

      ![Search for authorization object S_TABU_NAM](iam7.png)

  9. Select **`S_TABU_NAM`**, select **ACTVT** under **Authorization 0001** to check **`Change`** and **`Display`**.

      ![Select Change and Display](iam8.png)

 10. Click **TABLE** and add entity **`ZI_ERRORCODE###`**. A CDS entity can be specified for the field **TABLE**.

      ![Add entity](iam9a.png)

 11. To enable the user to see the changes in the app [**Business Configuration Change Logs**](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/5c6cf20499894f1083e80dba7c5963d4.html), add an additional instance of the authorization object with **`Display change documents`** for the activity and the table names.

      ![Select display change documents](iam9.png)

 12. Save the IAM app. For more information about IAM apps, see [here](https://help.sap.com/viewer/5371047f1273405bb46725a417f95433/Cloud/en-US/032faaf4f9184484ba9295c81756e831.html).



### Create business catalog


  1. In the overview section of the IAM app, click on **Create a new Business Catalog and assign the App to it**

      ![Create a new Business Catalog](iam0.png)
  2. Enter the following and click **Next >**.

      - Name: **`Z_ERROR_CODES_###`**
      - Description: **`Error Codes - Maintenance`**

     ![Business Catalog definition](bc3.png)

  3. Select a Transport Request and click **Finish**.

  4. Complete the wizard to create the Business Catalog App Assignment.

      ![Business Catalog App Assignment](bc5.png)

  5. In the Business Catalog, click **Publish Locally** to be able to test your app in the development system.

      ![Publish locally](bc10.png)


### Assign Business Catalog to Business Role and maintain restrictions


  1. To [create a Business Role](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/8ffb880eafec4078a1e5051227cb64b1.html) and assign it to your user, start the SAP Build Work Zone. Or right-click on your ABAP system and select **Properties**.

      ![Select properties](fiori.png)

  2. Choose **ABAP Development** and click on the **system URL**.

      ![Click on the system URL](fiori2.png)

  3. Log on with a user with the role `SAP_BR_ADMINISTRATOR - Administrator`

  4. Open the **Maintain Business Roles** app.

      ![Start Maintain Business Roles app](fiori4.png)

  5. Click **New** to create a new Business Role.

      ![Create new Business Role](fiori5.png)

  6. Create a new Business Role:
      - Business Role ID: **`ZBR_ERROR_CODES_EXPERT_###`**
      - Business Role Description: **`Error Codes Expert`**

      ![Business Role definition](fiori6.png)

      Click **Create**.

  7. Select **Assigned Business Catalogs** and click **Add**.

      ![Add Business Catalog](fiori7.png)


  8. Search for `Z_ERROR_CODES_###`, select it and click **OK**.

      ![Select Business Catalog](fiori8.png)

  9. Select **General Role Details** and set Access Category **Write, Read, Value Help** to **`Unrestricted`**. If you set Access Category **Write, Read, Value Help** to **`No Access`**, the user can only read the content, but not change it.

     ![Set Access Category to unrestricted](fiori9.png)

10. Select **Assigned Business Users** and click **Add**.

     ![Add Business User](fiori10.png)

11. Select the user responsible for maintaining the error codes and click **OK**

12. Click **Save** to save the Business Role


### Test yourself



---
