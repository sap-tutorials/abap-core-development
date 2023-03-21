---
parser: v2
auto_validation: true
time: 5
tags: [ tutorial>beginner, topic>sap-copilot, tutorial>license]
primary_tag: products>sap-copilot
---
# Assign OData Service to the SAP CoPilot Skill
<!-- description --> Learn how to assign a new Odata service of S/4HANA to the SAP CoPilot Skill and add Query intent as the custom scenario

<!---
## Prerequisites
 - [Create New Skill with SAP CoPilot Skill Builder](https://developers.sap.com/index.html)
-->

## You will learn
  - How to assign OData service to the skill of SAP CoPilot
  - How to select OData object for the skill
  - How to activate OData action for the skill

---

### Go to intent of the skill


Click **Select Intents**.
![button](copilot-01-1.png)


### Search OData service

A popup screen appears and you will assign an OData Service to the skill here.
![select odata service](copilot-02-1.png)
Click search button.
![select odata service](copilot-02-2.png)




### Select the OData service


Enter `SEPMRA_PROD_MAN` in the search field.

![select odata service](copilot-03-1.png)

Select `SEPMRA_PROD_MAN` and Click **Import**.
The OData Service is assigned to the skill.


### Select objects and actions


Under **Step 2: Select Objects and Actions**, Click **Product**.
![Product](copilot-04-1.png)

Enable **Query** by clicking the switch.
![Query](copilot-04-2.png)

You will see the **Query** is enabled like this.
![Query2](copilot-04-3.png)

![done](copilot-04-4.png)
Click **Done**




### Query intent was added 

You can see **Query Product** intent was added.

![Save](copilot-05-1.png)








---
