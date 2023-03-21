---
parser: v2
auto_validation: true
time: 35
tags: [tutorial>beginner, products>sap-business-technology-platform]
primary_tag: products>sap-btp--cloud-foundry-environment
---

# Set Up Monitoring in Your CF Environment
<!-- description --> Read about the perks of monitoring and how to get started

## Prerequisites
 - You have access to a multi-environment subaccount in the SAP BTP cockpit
 - You have Cloud Foundry enabled in your subaccount
 - You have at least one application in your subaccount that you would like to monitor

## You will learn
 - What monitoring is and why you should use it
 - How to start monitoring your applications with SAP BTP services
 - How to create a service instance of SAP Application Logging service for SAP BTP
 - How to bind the service to your application
 - How to interpret collected data
 
---
### Understand Monitoring
#### What is monitoring and why do you need it?

Monitoring, specifically log analysis, helps you manage your resources and provides you with an overall better experience of your applications and services. It lets you observe how your applications are performing and helps you act in case something is not running as smoothly as it should.



#### How log analysis works
 You connect your applications to a monitoring service. The selected monitoring service requests and receives data from your application in the form of logs. Those logs can contain information about disk capacity, traffic, downtimes, and more. To make use of this data, the logs are visualized in dashboards and reports, which give you comprehensive information about the performance of the applications. Alerts and automated actions can be set for certain events, so you don't have to constantly check your dashboards.

#### More information

"Monitor and Operate" is one of the four categories of the DevOps principles.
The other principles are "Plan & Setup", "Develop & Test" and "Deliver & Change".

Read more about monitoring in the context of your DevOps lifecycle here:

 - ["The DevOps Portfolio"] (https://blogs.sap.com/2019/10/23/the-devops-portfolio-of-sap-cloud-platform/)

 - ["Efficient DevOps"] (https://blogs.sap.com/2019/11/28/efficient-devops-with-sap-cloud-platform/).</ul>

#### Get started with SAP monitoring solutions for CF environments

For monitoring your applications in your CF environment, SAP is offering the SAP Application Logging service for Cloud Foundry. This service lets you stream logs of bound applications to a central logging stack. Using Elastic Stack, you are then able to visualize your logs in dashboards.

### Create a Service Instance of SAP Application Logging service

1. In your subaccount in the SAP BTP cockpit, go to **Services** > **Service Marketplace**.
2. Search for **Application Logging Service** and select the service.
3. Click on the **create** button.
4. From the menu of the wizard, pick a service plan. We recommend the free **lite** plan for testing purposes.
4. Enter a name for your service instance and click **next**.
5. You don't need to add any parameters. Click **next**.
6. Check the review and click **create**.
7. You have now created your service instance.



### Bind the Service to Your Application

1. In your subaccount, go to the space where you have just created the new service instance. There you will find a list of all of your applications.
(Please note: There must be at least one application in your space in order to bind the service to.)

2. Select the application you want to bind to the Application Logging service, then click on **Service Bindings** in the menu on the left.

3. Select **Bind Service** and follow the instructions of the wizard.
4. Repeat this step with all applications you want to bind to the Application Logging service.

>**Please note**: In order for the service to collect any data, your application needs to run for at least a bit. Otherwise there will be no logs.




### Have a Look at Bindings and Collected Data

1. In the navigation area of your space, choose **Applications** and click on the link to the application that you are logging.

2. Choose **Logs** from the navigation area. You will now see the latest logs for this application.

3. Click on **Open Kibana Dashboard** and log in. You can now navigate between different dashboards and perspectives of your logging data.

>**Please note**: The more applications you bind to the Application Logging service and the more data you collect, the more data you will see on the dashboards.

Kibana displays a set of pre-built dashboards that help you analyze your application as follows:

- Use the **Overview** dashboard (default) to understand the evolution of logs and basic KPI regarding failures, response time, and response size.
- Use the **Usage** dashboard to investigate the actual requests, their URLs, user, and component information.
- Use the **Performance and Quality** dashboard to investigate failures and response times.
- Use the **Network and Load** dashboard to investigate network traffic and payloads.
- Use the **Requests and Logs** dashboard to analyze the overall set of logs and requests as well as their involved components.
- Use the **Statistics** dashboard to see how many logs were shipped for each of your components as well as how many logs were dropped by the pipeline due to quota limitations.
- Use the **Metrics** dashboard to analyze CPU, memory, and disk usage of your applications.







---
