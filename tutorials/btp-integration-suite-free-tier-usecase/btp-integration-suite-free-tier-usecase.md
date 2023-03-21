---
parser: v2
auto_validation: true
time: 10
tags: [tutorial>beginner, software-product>sap-business-technology-platform, software-product>sap-btp--cloud-foundry-environment]
primary_tag: software-product>sap-integration-suite
author_name: Karunaharan V
author_profile: https://github.com/Karunaharan
---

# The Use Case for the Mission
<!-- description --> Understand the integration problem that you're going to solve before you get started

## You will learn
 - What is SAP Integration suite
 - The integration problem that you're going to solve

---

### What is SAP Integration Suite

SAP Integration Suite is an enterprise-grade integration platform as a service (EiPaaS) that allows you to smoothly integrate on-premise and cloud-based applications and processes with tools and prebuilt content managed by SAP.

SAP Integration Suite combines the integration capabilities such as Cloud Integration (Process Integration), API Management, Integration Advisor, Trading Partner Management, Integration Assessment, and Open Connectors into a cohesive and simplified toolkit for enterprise integrations. To provide a comprehensive integration experience, these services are not available separately, but only as part of the Integration Suite.

The Integration Suite includes all integration capabilities in simple service plans. To know more on these plans, see [Integration Suite](https://discovery-center.cloud.sap/#/serviceCatalog/f810c887-8d25-4942-9849-354837951066) service catalog.


### The Use Case

Using this scenario, you design and execute an integration flow that reads product details from a public product catalog (`WebShop`) for a given product identifier. Product details include data such as the product name, dimensions, and price, for example. To accomplish the scenario, you use SAP Integration Suite, and in particular, its capabilities *Cloud Integration* and *API Management*.

You use *Cloud Integration* to
  - reuse a standard integration package that solves the integration problem
  - deploy the integration artifact to a cloud-based runtime location

You use *API Management* to
  - expose the integration flow endpoint as an API
  - define how to access the API by assigning a dedicated predefined policy template that uses OAuth client credentials grant method
  - invoke the API and get the product details in a response.

  <!-- border -->![Use case](The-Usecase.png)


---
