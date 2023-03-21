---
parser: v2
auto_validation: true
time: 10
tags: [ tutorial>beginner, topic>sap-copilot, tutorial>license]
primary_tag: products>sap-copilot
---
# Configure intent with SAP CoPilot Skill Builder
<!-- description --> Learn how to configure Query intent with SAP CoPilot Skill Builder for the data access and its response visualization

<!---
## Prerequisites
 - [Assign OData to the Skill](https://developers.sap.com/index.html)
-->

## You will learn
  - How to configure the parameters of Query intent
  - How to add utterance for an intent of SAP Copilot
  - How to configure response visualization for an intent of SAP Copilot

---

### Select an intent


Select OData intent **Query Product**  

![intent](copilot-01-1.png)


### Intent basic configuration


Under **What parameters are used to Filter Product?**,
click  **+ Add Parameter**.  

![intent](copilot-02-1.png)


Select `Category` and `Supplier`.

![intent](copilot-02-2.png)

![intent](copilot-02-3.png)
Click **Done**.


### Configure entities


Click **Entities**.  

![entities](copilot-03-1.png)
Click **+ Add Parameter** under `Select Parameters that identifies Category uniquely from the user's utterance`.  

![add parameter](copilot-03-2.png)
Select Category and Click **Done**.  

![category](copilot-03-3.png)
Click **Done** again here.  

![category page](copilot-03-4.png)
You will see the screen like this  

![done](copilot-04-5.png)
Repeat the same thing to the `Supplier`.  

![done](copilot-04-6.png)
Choose `Supplier Name`.  

![done](copilot-04-7.png)


### Add utterance


Click **Utterances**.  

![Utterances](copilot-05-1.png)

Enter **`Show me products from category software`** and hit **Enter**.  

![Utterance](copilot-05-2.png)



### Configure response visualization


Click **Response Visualization**.  

![response visualization](copilot-06-1.png)

In **Thumbnail** field, enter thumbnail parameter.  

![thumbnail](copilot-06-2.png)

Click **Image Property**  and Select `Image(ProductPictureURL)`, Click **Add**.  

![add](copilot-06-3.png)

You can see the screen like this.  

![screen](copilot-06-4.png)

Click **Save**.  

![Save](copilot-06-5.png)









---
