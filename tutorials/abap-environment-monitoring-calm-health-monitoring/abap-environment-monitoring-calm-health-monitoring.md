---
parser: v2
auto_validation: true
time: 30
tags: [ tutorial>intermediate, tutorial>license, software-product>sap-btp--abap-environment, programming-tool>abap-development]
primary_tag: topic>cloud-operations
author_name: Julie Plummer
author_profile: https://github.com/julieplummer20

---

# Monitor An SAP BTP ABAP Environment Service Using SAP Cloud ALM (CALM)
<!-- description -->
Configure health monitoring and real user monitoring for an SAP BTP ABAP Environment service using SAP Cloud ALM.

## Prerequisites
- SAP Cloud ALM instance
- SAP BTP ABAP Environment instance with the Administrator role

**CAUTION**: To complete this tutorial, you need a customer license (i.e. license number). This applies to customers, partners, and SAP employees. 


## You will learn
  - How to configure outbound communication from your SAP BTP ABAP Environment  using the communication scenario **`SAP_COM_0276`**
  - How to configure communication from your SAP BTP ABAP Environment instance to the SAP Cloud ALM service using the communication scenario  **`SAP_COM_0527`**
  - How to configure the use cases you want for SAP Cloud ALM



## Intro
If you run SAP BTP, ABAP environment, as one part of a hybrid landscape with on-premise and cloud systems, you might already have a central monitoring and alerting infrastructure in place. For example, the service offering SAP Focused Run (FRUN) is designed specifically for businesses that need high-volume system and application monitoring, alerting, and analytics. This is particularly relevant for hybrid landscapes including on-premise SAP solutions. For cloud-centric customers, we offer SAP Cloud ALM as an application lifecycle management offering.

If you use SAP Cloud ALM  as your monitoring and alerting infrastructure, you can integrate monitoring of the ABAP environment into your existing infrastructure. Using the health monitoring in SAP Cloud ALM, you can also watch whether your ABAP environment is still up and running and whether any exceptional situations occurred. In addition, you can use the real-user monitoring to monitor requests coming from business users and integration monitoring to watch the communication between integrated systems.

For more information, see SAP Help Portal: [Integration in Central Monitoring and Alerting](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/8d6e2e78f77540d6836cc63eea121966.html)

Throughout this tutorial, objects name include a prefix, such as **`Zxx`** or suffix, such as **`XXX`**. Always replace this with your group number or initials.

<!-- border -->
![step0-overview](step0-overview.png)

<!-- border -->
![step0-overview2](step0-overview2.png)

---


### Configure destination

In the BTP Cockpit, you need to create a destination to SAP Cloud ALM if you have not already done so.

1.	Create a service key for your SAP Cloud ALM service and download it; you will need it later - that is, the SAP Cloud ALM service mentioned in the prerequisites above.

    <!-- border -->
    ![step1c-calm-service-already-created](step1c-calm-service-already-created.png)

2. Navigate to your **Subaccount** and choose **Destinations > New Destination**.

    <!-- border -->
    ![step1c-navigate-to-destinations](step1c-navigate-to-destinations.png)

3. Configure the destination to your SAP Cloud ALM service using the credentials in the service key (from step 1.3).

    <!-- border -->
    ![step1b-destination-to-calm](step1b-destination-to-calm.png)


### Create Communication Arrangement for Application Monitoring (SAP_COM_0527)

1. Again, choose **New**, then choose **Create**.

    <!-- border -->
    ![step3a-comm-arr-create-SAP_COM_0527](step3a-comm-arr-create-SAP_COM_0527.png)

2. Create a New **Communication System**.

    <!-- border -->
    ![step3b-comm-system-new-cutout](step3b-comm-system-new-cutout.png)

3. From the dropdown list, choose the **Name** for your SAP Cloud ALM service, that you configured in step 1.3.

    <!-- border -->
    ![step3c-comm-system-name](step3c-comm-system-name.png)

4. Choose the use cases you want to enable.

    <!-- border -->
    ![step3d-configure-use-cases](step3d-configure-use-cases.png)

5. Configure the job execution details as follows and choose **Save**, then save the communication arrangement.

    <!-- border -->
    ![step3e-configure-job-exec-details](step3e-configure-job-exec-details.png)

Your SAP BTP ABAP environment is registered in your SAP Cloud ALM instance. However, it could take **up to one hour** before the first monitoring data is pushed to your  service.

<!-- border -->
![step3e-result-calm-health-monitoring](step3e-result-calm-health-monitoring.png)


### Test yourself







---
