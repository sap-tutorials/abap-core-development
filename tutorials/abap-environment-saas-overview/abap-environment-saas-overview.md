---
author_name: Sumana Vasuki
author_profile: https://github.com/Sumana-vasuki
parser: v2
auto_validation: true
time: 5
tags: [ tutorial>beginner, programming-tool>abap-development, tutorial>license]
primary_tag: software-product>sap-btp--abap-environment
---

# Get an Overview of Building and Maintaining SaaS Applications on SAP BTP, ABAP Environment

<!-- description -->  Overview of developing and deploying custom SaaS applications on SAP Business Technology Platform, ABAP Environment

## Prerequisites

- You've purchased entitlements that are necessary for the account setup. See [Prepare](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/3bf575a3dc5043f895f8bd411d2a86a1.html?locale=en-US&version=Cloud#loio4338854e3133407abb47d3a281dbd1e1)
- You've registered a namespace at SAP, for example /NAMESPC/. See [Register a Namespace](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/3bf575a3dc5043f895f8bd411d2a86a1.html?locale=en-US&version=Cloud#loiocc5a3c6f78cf4889960c314dd09a5060)
- You've registered your add-on at SAP, for example /NAMESPC/PRODUCTX. See [Register a Product](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/dc15fb4ebab5453fa4641b98190b1f85.html?locale=en-US&version=Cloud)
- You've created a technical communication user, for example with the credentials ID `TechUserAAKaaS`.
- You've created a technical platform user as a space member in Cloud Foundry, for example with the credentials ID `CFPlatform`.
- You've configured an ASP\_CC destination for cloud controller access in the 05 Provide subaccount in the global account for development based on the credentials of the technical Cloud Foundry platform user. See [Create a Destination for the Cloud Foundry Cloud Controller Access](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/35b5acbb32024aa6b90a22e9f957a9f6.html?locale=en-US&version=Cloud).

## You will learn

- About SaaS enablement of an add-on product on SAP Business Technology Platform, ABAP Environment
- The End-End process for building and deploying an add-on product on ABAP Environment and providing it as multitenant application

## Intro

This tutorial is intended as an overview of the necessary steps, and provides a sneak-peak into the overall process. Detailed steps on how to achieve this will be covered in further tutorials of this mission.

---

### Learn about the global accounts and subaccounts needed

The following figure shows the system landscape recommended for developing, operating and maintaining SaaS applications on SAP Business Technology Platform, ABAP Environment

![System Landscape](systemLandscape.png)

1. Global Account for Development

- 01 Develop - Holds the development system (DEV) used for continuous development and the correction system (COR) used to develop bug fixes and patches for already released versions of your add-on
- 02 Test - Contains a test system (TST) for testing the changes released from development and the quality assurance system (QAS) to test the bug fixes and patches for already released versions of your add-on  
- 03 Build/Assemble - Holds the system created for the add-on assembly. Software components are imported into this system, ATC checks executed, delivery packages for the add-on created
- 04 Build/Test - Holds the system created for add-on installation test performed after add-on build
- 05 Provide - Contains a production like system (AMT) to test multi-tenancy aspects of the SaaS solution (client separation, data isolation...)
- 06 Consume - Consumer subaccounts for facilitating production like tests

![Global Account For Development](GlobalAccountForDev.png)

2.Global Account for Production  

- 05 Provide - Provider subaccount for production use - Contains various services for SaaS enablement, ABAP system (AMT) provisioned via ABAP Solution service
- 06 Consume - Consumer subaccounts for production - Each of your customers is assigned a dedicated consumer subaccount
![GlobalAccountForProd](GlobalAccountForProd.png)

### ABAP Development/UI Development

1.In the development system DEV of your 01 Develop subaccount create a software component with name pattern 'NAMESPC/COMPONENT', where the 'NAMESPC' is the namespace you registered as part of the prerequisites, and COMPONENT is a name of your choosing. See [Manage Software Components](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/3dcf76a072c9450eb46b99db947dab46.html?locale=en-US&version=Cloud). Implement your custom business services with the ABAP RESTful Application Programming Model in this software component.

For more information on software components, see [Software Components](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/58480f43e0b64de782196922bc5f1ca0.html?locale=en-US&version=Cloud).

For more information on the ABAP RESTful Application Programming Model, see [ABAP RESTful Application Programming Model](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/33a301e3fff5404e89f090910f7bd978.html?locale=en-US&version=Cloud).

2.Subscribe to the SAP Business Application Studio in the 01 Develop subaccount. See [Subscribing to SAP Business Application Studio](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/0a9b42e8795d4f7b8fc196a6fde0b3f2.html?locale=en-US&version=Cloud).

3.For more information on how to develop a user interface for the application and deploy it to a ABAP system, see [Develop an SAP Fiori Application UI and Deploy it to ABAP Using SAP Business Application Studio](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/eaaeba48e5e04949855f2763477cd557.html?locale=en-US&version=Cloud).

4.To automate testing of your application and ensure quality, consider writing robust tests for your ABAP Code. For more information see [Ensuring Quality of ABAP Code](https://help.sap.com/docs/btp/sap-abap-development-user-guide/ensuring-quality-of-abap-code?locale=en-US).

### Test your application

After you have successfully developed your application, created a UI, and deployed it to your ABAP system, you will need to Clone or Pull the changes in your software component "/NAMESPC/COMPONENT" into your TST system in the 02 Test subaccount and manually test your changes.

See [How to clone software components](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/18564c54f529496ba420d4c83545a2ce.html?locale=en-US&version=Cloud).

Run ABAP Test Cockpit checks in DEV and/or TST systems. See [ABAP Test Cockpit in the cloud](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/22c26ff27b9f44b7b7229a01e8e8ed25.html?version=Cloud) to understand what is already possible.  

### Build The First Version of Your Add-on Product

Once the application is developed and tested, the next step is to build a first version of your add-on using the [ABAP Environment pipeline](https://www.project-piper.io/pipelines/abapEnvironment/introduction/).

The result of a successful build is an add-on target vector that can be installed/updated in ABAP systems of the type `abap/saas-oem`

This is explained in a future tutorial of this mission. Steps at a glance.

1.To capture the current state of your software component, create a branch in development system DEV of your 01 Develop subaccount. See [How to Work with Branches](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/6b2f0bfc14cb47ef888f01784c92e1bf.html?locale=en-US&version=Cloud).
>We recommend naming this first branch v1.0.0 and to create a new branch when updating the application with a support package (v1.1.0) or a new release (v2.0.0).

2.Use the Build Product Version app of the landscape portal to build and publish your add-on product
![AddOnBuildPipeline](AddOnBuildPipeline.png)

### Implement Multitenant Application as MTA and deploy to Cloud Foundry

Steps at a glance (detailed steps in further tutorials of this mission):

1. To prepare the creation of your multitenant application, navigate to your dev space in SAP Business Application Studio. Select "Start from Template" > Basic Multi-target Application.

2. [Update the created MTA descriptor and MTA extension descriptors with suitable configuration](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/ca0cc10a2d1249ebbde41f778d1932be.html?locale=en-US&version=Cloud).

3. Develop and configure the [Approuter Application](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/44dbd0ae4d4b4d9c9c8371d711c22bfe.html?locale=en-US&version=Cloud).

4. Build the MTA project and [deploy it to Cloud Foundry](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/faf51067caf348959dc9cb92a963af38.html).

![MultiTenantApplicaton](MultiTenantApplication.png)

### Provide the SaaS solution to consumers

For a consumer to be able to access the SaaS solution after subscription, a consumer-specific route pointing to the approuter application needs to be created. See [Create Routes](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/9fddeea396b34b528bc8d286f3d5d9cf.html?locale=en-US&version=Cloud).

This route needs to match the property TENANT\_HOST\_PATTERN defined in the "mta.yaml" file.

1. Create a new route in the space of multitenant application in the 05 Provide subaccount
2. Assign the route to the deployed approuter application
3. To enable consumer access, subscribe to the solution from the 06 consume subaccount
4. To enable creation of initial administrator user in the consumer tenant, assign the role collection <app\_name>-admin with the role `SolutionAdmin` to your user. (the creation of a role collection is part of the prerequisites for this tutorial)
5. Open the application URL provided by the subscription with the user you wish to onboard as initial administrator user. This is the same user that was assigned to the <app\_name>-admin role collection. After triggering initial user onboarding, you are redirected to the SAP Fiori launchpad of the SaaS solution

![ConsumerSubaccount](ConsumerSubAccount.png)

### Maintain the SaaS solution

The Landscape Portal acts as a central tool to allow service providers to perform lifecycle management operations

- Install namespaces in various ABAP systems of your Landscape using the [Maintain Namespaces application](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/5456007ac4d04cb98b52b41f8c2d4a71.html?locale=en-US&version=Cloud) of the landscape portal.

- The [Register Systems for Pre-Upgrade app](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/100be99ac7064dfca43f0a0750d90d5b.html?locale=en-US&version=Cloud) allows you to select specific test or development systems for an early upgrade. These systems will be upgraded two weeks before the official roll-out, giving you ample time to test your solution prior to the actual upgrade.

- The [Register Product app](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/dc15fb4ebab5453fa4641b98190b1f85.html?locale=en-US&version=Cloud) provides information about which global accounts are already registered for an available product and enables you, as a provider, to create new products and register your global accounts for installation of existing products.

- The [Check Product Version app](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/0da158a663c2431099e9ea1b3cec6e9a.html?locale=en-US&version=Cloud) lets you view the results of product version delivery infrastructure checks to see if your product version is ready for delivery. The app shows results for checks of the product version, its components and respective packages.

- As a provider, you can use the [Update Product Version app](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/32c4f7d3f0224fc2be3a1103297db59f.html?locale=en-US&version=Cloud) to update the version of your product on specific systems.

- The [Build Product Version app](https://help.sap.com/docs/btp/sap-business-technology-platform/build-product-version) lets you easily trigger the build of new product versions via pipelines based on templates that you can configure for different use cases such as new release deliveries, support packages or hotfixes/patch deliveries.

- The [Operations Dashboard](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/0a3a7359c24341b5bac61652b4858bff.html?locale=en-US&version=Cloud) gives an overview of all processes that have been triggered in the Landscape Portal to help you monitor your operations.

- The [Systems Overview App](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/99c975e077bc447f9049056256a7e46c.html?locale=en-US&version=Cloud) provides an overview of all the systems and tenants in the global account where Landscape Portal is being accessed. The app can also be used to [create support users](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/b31712cc9dbf44fb8a603ab4be6f8d28.html?locale=en-US&version=Cloud) for access to consumer tenants for troubleshooting bugs/issues faced by the consumer.

### Set up maintenance system landscape

Maintaining a solution involves development of bug fixes.  Bug fixes are not implemented as part of the development code line (systems DEV and TST) but in a separate correction code line (COR) with separate systems to develop and test these corrections.

![MaintenanceSysLandscape](MaintenanceSysLandscape.png)

According to what is mentioned in the prerequisites, the ABAP systems that you use for development, testing, and add-on assembly are of type ABAP/standard and made available via entitlements. As a SaaS solution operator, you have to assign service entitlements to different subaccounts according to the following structure:

| Global Account| Sub-Account | Space | Entitlement | ABAP Systems|
|---------------|-------------|-------|-------------|-------------|
| Global Account for Development|01 Develop|Develop |1x ABAP/standard | COR|
|                       |      |    |`ABAP/hana_compute_unit (standard: 2)`||
|                       |      |    |`ABAP/abap_compute_unit (standard: 1)`||
|                                 | 02 Test | Test |1x ABAP/standard | QAS|
|                                 |         |        |`ABAP/hana_compute_unit (standard: 2)`||
|                                 |       |     |`ABAP/abap_compute_unit (standard: 1)`||

The service parameter `is_development_allowed` is used to differentiate between development and test systems.

>For development in the ABAP correction system COR, you, as a SaaS solution operator, create a system in subaccount 01 Develop in the development space. For correction system COR, set parameter `is_development_allowed = true`.
>For testing in the ABAP quality assurance system QAS, you, as a SaaS solution operator, create a system in subaccount 02 Test in the test space. For quality assurance system QAS, set parameter `is_development_allowed = false`.

Users in the ABAP correction system COR might be locked and need to be unlocked for development of corrections.

For more information around versioning and branches, refer the [help documentation](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/9482e7eef4634cb993a4ae296b2029fa.html#versioning-and-branches).

For more information around add-on package types, refer the [help documentation](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/9482e7eef4634cb993a4ae296b2029fa.html#loio398be26971414fbb91cbe3b0a3cb8a26).

### Test Yourself

---
