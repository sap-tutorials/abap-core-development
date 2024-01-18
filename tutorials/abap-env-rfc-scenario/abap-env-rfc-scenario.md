---
parser: v2
auto_validation: true
time: 30
tags: [ tutorial>intermediate, software-product>sap-btp--abap-environment, software-product>sap-business-technology-platform, software-product-function>sap-s-4hana-cloud--abap-environment, tutorial>license]
primary_tag: programming-tool>abap-development
author_name: Julie Plummer
author_profile: https://github.com/julieplummer20

---

# Get Data from a Remote System Using an OData Service: Scenario
<!-- description --> Call a remote function module located in an on-premise system, such as a SAP S/4HANA System, from the ABAP Environment

## Prerequisites 

- **IMPORTANT**: This tutorial cannot be completed on a trial account. If you want to explore some of the concepts of this mission on a trial account, using OData or SOAP rather than RFC, see the following workshop: [SAP BTP, ABAP Environment: Connectivity and Integration](https://github.com/SAP-samples/teched2020-DEV268).
- You have set up SAP Business Technology Platform (BTP), ABAP Environment, for example by using the relevant booster: [Using a Booster to Automate the Setup of the ABAP Environment](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/cd7e7e6108c24b5384b7d218c74e80b9.html)
- **Tutorial**: [Set Up SAP BTP, ABAP Environment and create Your First Console Application](abap-environment-trial-onboarding), for a licensed user, steps 1-2
-	You have rights to call the appropriate resources on an ABAP on-premise system. See step 2.4 for details. Examples of such on-premise systems include:
    - [AS ABAP developer edition, latest version](https://blogs.sap.com/2019/07/01/as-abap-752-sp04-developer-edition-to-download/) or:
    - [SAP S/4HANA 1809 fully activated appliance](https://blogs.sap.com/2018/12/12/sap-s4hana-fully-activated-appliance-create-your-sap-s4hana-1809-system-in-a-fraction-of-the-usual-setup-time/) or:
    - [The SAP Gateway Demo System (ES5)](https://blogs.sap.com/2017/12/05/new-sap-gateway-demo-system-available/)
- You have connected **SAP Cloud Connector**, to your BTP subaccount
- You have assigned the business role **`SAP_BR_DEVELOPER`** to your user; you will need it to create communication artifacts

  
## You will learn 
  
  - How to create a secure RFC connection between your SAP BTP, ABAP Environment and an on-premise SAP System, e.g. SAP S/4HANA
  - How to test the connection using an ABAP handler class

This tutorial mission was written for SAP BTP ABAP Environment. However, you should also be able to use it in SAP S/4HANA Cloud Environment in the same way.

Throughout this tutorial, replace `000` with your initials or group number.

**The challenge:**

There are two challenges when setting up connectivity between the SAP BTP, ABAP Environment and an on-premise ABAP System:

- The ABAP Environment "lives" in the Internet, but customer on-premise systems are behind a firewall
- Remote Function Call (RFC) is not internet-enabled

**The solution:** 

Set up a secure connection from the on-premise system to the SAP BTP, ABAP Environment.


**Technical information:**

1. SAP Cloud Connector opens a tunnel connection to the ABAP environment tenant using its public tenant URL
2. After the tunnel is established, it can be used for actual data connection using the RFC or HTTP(S) protocols. 


### Test yourself


### More Information

This tutorial is based on an excellent blog post by André Fischer:

- [How to call a remote function module in your on-premise SAP system...](https://blogs.sap.com/2019/02/28/how-to-call-a-remote-function-module-in-your-on-premise-sap-system-from-sap-cloud-platform-abap-environment/)


For OData services in general:

- Mission: [Take a Deep Dive into OData](mission.scp-3-odata)

For SAP Gateway in general, see:

- [OData service development with SAP Gateway using CDS](https://blogs.sap.com/2016/06/01/odata-service-development-with-sap-gateway-using-cds-via-referenced-data-sources/) - pertains to on-premise Systems, but contains lots of useful background information on the relationships between CDS views and OData services

For connectivity in this context, see:
- SAP Help Portal: [SAP Cloud Connector](https://help.sap.com/docs/connectivity/sap-btp-connectivity-cf/cloud-connector)

- SAP Help Portal: [RFC Communication via Communication Arrangements](https://help.sap.com/docs/btp/sap-business-technology-platform/rfc-communication-via-communication-arrangements)

For SAP Business Technology Platform (BTP):

- SAP Help Portal: [What is SAP Business Technology Platform (BTP)](https://help.sap.com/docs/btp/sap-business-technology-platform/btp-basic-platform-concepts)

- SAP Help Portal: [Getting Started With a Customer Account](https://help.sap.com/docs/btp/sap-business-technology-platform/getting-started-with-customer-account-in-abap-environment) - If you use the booster, these steps are performed automatically for you, but you may be interested in the background information
  
---