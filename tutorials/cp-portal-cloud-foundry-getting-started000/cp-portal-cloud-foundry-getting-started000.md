---
parser: v2
auto_validation: true
time: 5
tags: [ tutorial>beginner, products>sap-cloud-platform, products>cloud, products>sap-fiori]
primary_tag: products>sap-cloud-platform-portal
---

# Prepare the Portal Environment for Creating Sites
<!-- description --> To get started with building a Portal site, administrators must perform the required onboarding steps.

## Prerequisites
  - You've created a SAP Cloud Platform trial account in the Cloud Foundry environment on an AWS Data Center. You can create a trial account using this link: [Create a trial account](https://cockpit.hanatrial.ondemand.com).
  - You can also use a subaccount in your SAP Cloud Platform global account. If you don't have a subaccount in the Cloud Foundry environment, refer to this topic: [Initial Setup](https://help.sap.com/viewer/ad4b9f0b14b0458cad9bd27bf435637d/Cloud/en-US/fd79b232967545569d1ae4d8f691016b.html).


## You will learn
  - How to subscribe to the Portal Service
  - How to assign users to the `Super_Admin` role so that they can design Portal sites
  - How to access the Portal service

## Intro
In this group of tutorials our goal is to create an attractive Portal site using the SAP Cloud Platform Portal service. But before we can do this, there are some steps you need to do in the SAP Cloud Platform cockpit.

### Subscribe to the Portal service


Before you can access the Portal service, you first need to subscribe to it.

1. [Log onto SAP Cloud Platform](https://cockpit.hanatrial.ondemand.com) and click **Enter Your Trial Account**.

    > If this is your first time accessing your trial account, you'll have to configure your account by choosing a region (select the region closest to you). Your user profile will be set up for you automatically.

    > Wait till your account is set up and ready to go. Your global account, your subaccount, your organization, and your space are launched. This may take a couple of minutes.  

2. Click **Continue**.

3. Click on the **trial** tile to navigate to your trial subaccount in the SAP Cloud Platform cockpit. If you are using your own subaccount, you can select it instead.

4. Click **Subscriptions** from the side menu.

5. Enter `Portal` in the search box and click the **Portal** service tile.

6. Click **Subscribe** and wait for the status to change to **Subscribed**.




### Add yourself to the Super_Admin role


To be able to do administrative tasks in the Portal you must be assigned to the `Super_Admin` role. In this step, you will first create a role collection and then you'll assign yourself to the `Super_Admin` role.

1. Click on your subaccount again using the breadcrumbs at the top.

2. Click **Security > Role Collections** from the side menu.

3. Click **New Role Collection** and then name your role collection `Administrator`. Then click **Save**.

4. Click your role collection to open the **Roles** screen and then click **Add Role**.

5. Select the following values:

    |  Property     | Value
    |  :------------- | :-------------
    |  Application Identifier           | **`portal-cf-service!<id>`** <div>&nbsp;</div> Note that the **Application Identifier** has an ID at the end - just make sure that you choose the value with this format: **portal-cf-service!`<id>`**
    |  Role Template           | **`Super_Admin`**
    |  Role    | **`Super_Admin`**


6. Click **Save**.

7. Go back to your subaccount (you can use the breadcrumbs at the top of your screen).

8. Click **Security > Trust Configuration** from the side menu.

9. Click the `SAP ID Service`.

10. Enter your email address and then click **Show Assignments**.

11. If your user is not part of the SAP ID Service you will get a popup. Click **Add User**.

12. Click **Assign Role Collection**.  In the dialog box, select the `Administrator` role collection that you defined above and then click **Assign Role Collection**.

You have now been assigned to the `Super_Admin` role and you can access the Portal service and carry out all of your admin tasks.



### Access the Portal service


You are now ready to access the Portal service.  

1. Click on your subaccount.

2. Click **Subscriptions** from the side panel.

    You'll see that you are now subscribed to the Portal service.

3. Click **Go to Application** on the **Portal** tile.

4. Add your credentials if you are prompted to do so.

   The Portal service opens with the Site Directory in focus. This is where you create and manage the sites that you create for this subaccount.



