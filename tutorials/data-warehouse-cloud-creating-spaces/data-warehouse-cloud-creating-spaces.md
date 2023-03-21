---
parser: v2
auto_validation: true
time: 5
tags: [ tutorial>beginner, products>sap-data-warehouse-cloud]
primary_tag: products>sap-data-warehouse-cloud
---

# Create a Space in SAP Data Warehouse Cloud
<!-- description --> A Space is like a virtual team or office space. These virtual workspaces are designed for an individual or a group of users to perform their data modeling and data integration. Spaces are totally isolated from each other and can be assigned quotas for available disk space, CPU usage, runtime hours, and memory usage.

## Prerequisites
 - You have [familiarised yourself with the SAP Data Warehouse Cloud interface.](data-warehouse-cloud-intro-interface)

## You will learn
  - How to create a Space
  - How to manage and monitor your Space

## Intro
Add additional information: Background information, longer prerequisites

---

### Create a Space


1.	To create a space, click on the space management icon on the bottom left, and click on the plus symbol on the top right.

    ![Create Space](Picture1.png)

2.	Enter a name for your space. The Space ID will auto populate. In this example, let's call our space Best Run Bikes.

    ![Space Name](Picture2.png)

3.	Click on Create, and you've successfully created your Space in SAP Data Warehouse Cloud.


### Understand your Space


In the Space Management screen, each Space is represented by a tile. The Spaces tiles can show a red, green, or blue color table.


•	Green spaces are performing optimally.


•	Red spaces are overactive and need more resources allocated to perform optimally.


•	Blue spaces are under active.

Some resources that aren't being used by a blue space could be reallocated to a red space, for example. You also have the option to monitor a space and see detailed information about disk use and in memory usage. This is done by clicking on the three dots next to your space name.

![Space Overview](Picture3.png)

To see an overview of your space settings, simply select the Space, and this takes you to the overview, where you can monitor your space and see a more detailed view of the same. You can fine tune your disk-space and memory allocations as per your requirements if you wish. For this example, we accept the minimum configuration.

![Space Details](Picture4.png)

Now that you've created your Space, you can now move ahead and assign users to your Space.



---
