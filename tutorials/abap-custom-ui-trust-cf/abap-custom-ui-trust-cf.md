---
parser: v2
auto_validation: true
primary_tag: topic>abap-development
tags: [  tutorial>beginner, tutorial>license, topic>abap-development, topic>abap-extensibility  ]
time: 25
author_name: Peter Persiel
author_profile: https://github.com/peterpersiel
---
<!--DONE with BTP Trial and axoznozzp -->
# Set Up Trust Between SAP Cloud Identity Services and SAP BTP Subaccount

<!-- description -->Set up trust between SAP Cloud Identity Services - Identity Authentication and SAP Business Technology Platform subaccount for secure communication via SAML 2.0 with SAP S/4HANA Cloud.

## You will learn

- How to set up SAP BTP subaccount for secure communication (with Security Assertion Markup Language = SAML 2.0)
- How to set up SAP BTP subaccount on SAP Cloud Identity Services for secure communication
- How to get necessary information from your SAP BTP subaccount and your SAP Cloud Identity Services tenant to set up the mutual trust between them

## Prerequisites

- Your user needs Administrator access to your **SAP Business Technology Platform** (aka SAP BTP) subaccount
- Your user needs Administrator access to your **SAP Cloud Identity Services tenant**

#### Glossary

- *Identity*: individual people, but also computers, services, computational entities like processes and threads, or any group of such things
- *Identity Provider*: system entity that creates, maintains, and manages identity information for identities
- *Identity Authentication*: process of authenticating an identity
- *SAP Cloud Identity Services*: SAP's solution to enable identity authentication
- *SAP Cloud Identity Services tenant*: a customer's instance of the services
- *SAP Cloud Identity Services console*: Web application to configure your tenant

#### Additional Information

- Tutorial last updated in September 2025
- Documentation: [SAP Cloud Identity Services](https://help.sap.com/docs/cloud-identity-services/cloud-identity-services/landing-page?version=Cloud)

>Be aware that in case of an integration with SAP S/4HANA Cloud the used Identity Authentication for the SAP BTP subaccount should be the very same as the one used for the SAP S/4HANA Cloud system.
>
>Your SAP S/4HANA Cloud system you got already delivered by SAP comes with a configured trust between it and your SAP Cloud Identity Services tenant. Now you will configure the trust between that and your SAP BTP subaccount on your own.
>
>![SAP S/4HANA Cloud and SAP BTP subaccount share same Identity Provider](trust_IAS_SCP.png)

---
<!-- referred to in https://developers.sap.com/tutorials/abap-custom-ui-bas-connect-s4hc.html -->

### Get SAML metadata of SAP BTP subaccount

To set up the trust from Identity Authentication to the SAP BTP subaccount you need the subaccount's SAML metadata.

<!--border-->
![Enter SAP BTP Trust Configuration and get metadata](btp-open-trust-config-get-metadata.png)

1. Enter the SAP BTP subaccount's cockpit as an administrator and expand the **Security** area.

2. Open **Trust Configuration**.

3. Click **Download SAML Metadata**.

The metadata will be downloaded as XML file.

### Enter SAP Cloud Identity Services administration console

Open the SAP Cloud Identity Services administration console with its URL which follows the pattern:

`https://<YOUR_TENANTS_ID>.accounts.ondemand.com/admin`

The Tenant ID is an automatically generated ID by the system. The first administrator created for the tenant receives an activation e-mail with an URL in it. This URL contains the tenant ID.

SAP Cloud Identity Services administration console entry screen looks (depending on authorizations) like this

<!--border-->
![Enter SAP Cloud Identity Services administration console](IAS_entryScreen.png)

### Add SAP BTP subaccount as an application

The SAP BTP subaccount is represented in SAP Cloud Identity Services as Application.

Choose **Applications & Resources** (1) and go to **Applications** (2). Click **Create** (3) on the left hand panel and enter a **Display Name** (4) to represent your SAP BTP subaccount. **Create** (5) the application.

<!--border-->
![Add SAP BTP subaccount as application](IAS_addApplication.png)

### Configure application's trust with SAP BTP subaccount

1. The newly created application will be shown, choose **SAML 2.0 Configuration** and then the option **Load from File**.

    <!--border-->
    ![Configure application' s SAML 2.0 trust with SAP BTP subaccount](IAS_openSamlConfig.png)

2. Browse for the SAML metadata XML file of your SAP BTP subaccount that you downloaded before and upload it. All the needed properties will be automatically fetched from the XML file and saved in the SAML 2.0 configuration of the application.

    <!--border-->
    ![Upload SAP BTP subaccount' s metadata](IAS_subaccountMetadata.png)

### Set application's Subject Name Identifier

Now you have to configure which attribute is used to identify users during `SAML2.0` secure communication. By default this is **`User ID`**, but as SAP S/4HANA Cloud by default works with **`Login Name`** it shall be switched to that.

1. Still being in your application's Trust settings select **Subject Name Identifier**.

    <!--border-->
    ![Open Subject Name Identifier configuration](IAS_openSubjectNameID_attributeConfig.png)

2. Under **Primary Attribute** use **Identity Directory** as **Source**, choose **Login Name** as **Value** and save your changes.

    <!--border-->
    ![Set Login Name as application' s Subject Name Identifier](IAS_subjectNameID_attribute_setLoginName.png)

### Configure application's Default Identity Provider

As most common use case the SAP Cloud Identity Services - Identity Authentication does not act as Identity Provider itself but as proxy for an already existing corporate identity provider. This has to be set now.

Still being in your application's Trust settings scroll down and open **Conditional Authentication**.

<!--border-->
![Open application' s identity provider configuration](IAS_openIdP_config.png)

Under **Default Authenticating Identity Provider** select your corporate identity provider as **Default Identity Provider** and click **Save**.

<!--border-->
![Set identity provider](IAS_setCorporateIdP_asIdP.png)

### Get SAML metadata of SAP Cloud Identity Services tenant

To set the SAP Cloud Identity Services tenant as trusted identity provider in the SAP BTP subaccount next, you need to get its SAML metadata first.

<!--border-->
![Open SAP Cloud Identity Services tenant's settings - SAML 2.0 configuration](IAS-tenant-settings-SAML-config.png)

1. Choose **Applications & Resources**

2. Switch to **Tenant Settings**

3. Go to **Single Sign-On** section

4. Open **SAML 2.0 Configuration**

5. Click the **Download Metadata file** button

    <!--border-->
    ![Button to start download of SAML 2.0 Metadata](IAS-download-metadata-button.png)

6. In the pop-up that opens, use **Default certificate** and press the **Download** button.

    <!--border-->
    ![Pop-up to download SAML 2.0 Metadata](IAS-download-metadata-popup.png)

>Alternatively you can open the metadata XML by entering your tenant's web address for it which follows pattern `https://<YOUR_TENANTS_ID>.accounts.ondemand.com/saml2/metadata` and saving that XML to a file.

### Add SAP Cloud Identity Services tenant as SAP BTP subaccount's trusted identity provider

Switch back to your SAP BTP cockpit trust configuration.

Choose **New SAML Trust Configuration** to add a trusted identity provider.

<!--border-->
![Click New SAML Trust Configuration](btp-new-trust-config-button.png)

Upload the metadata XML file of your SAP Cloud Identity tenant in the **Metadata File** field, give a **Name**, as for example the tenant id. **Save** your changes.

<!--border-->
![Upload identity tenant' s metadata as trusted identity provider and save](btp-new-trust-config.png)

### Test yourself
