---
parser: v2
auto_validation: true
primary_tag: topic>abap-extensibility
tags: [  tutorial>beginner, tutorial>license, topic>abap-extensibility, topic>cloud, products>sap-s-4hana ]
time: 10
author_name: Peter Persiel
author_profile: https://github.com/peterpersiel
---
<!--DONE with FYZ/100 -->
# Adapt the UI of a Business Object

<!-- description -->Adapt the UI of a business object inside SAP S/4HANA Cloud, shown for the generated UI of a Custom Business Object

## You will learn  

- How to adapt a UI for all users in a system

## Prerequisites  

- **Authorizations:** Your user needs a business role with business catalogs **Extensibility - Custom Business Objects** (ID: `SAP_CORE_BC_EXT_CBO`) and **Extensibility - Key User Adaptation Customizing** (ID: `SAP_CORE_BC_EXT_FLX_CUS_PC`) in your **SAP S/4HANA Cloud** system

#### Additional Info

- In the preceding tutorials you created a custom business object with a simple data structure and its persistence. Afterwards you generated an UI for this business object and exposed it as a Fiori Launchpad application. As the generated User Interfaces only lists all fields of a business object node, adapting the UI might be necessary to improve usability of it.
- The **UI Generation** done in a previous tutorial and the **UI Adaptation** shown in this tutorial are so called **In-App Extensibility** features done completely inside a S/4HANA Cloud system. They are key user functionality with limited possibilities. In contrast to UI Generation and Adaptation, a UI with all SAPUI5 options can be developed with **SAP Business Application Studio** (see tutorial group [Create an SAP Fiori App and Deploy it to SAP S/4HANA Cloud](group.abap-custom-ui-s4hana-cloud)).
- Tutorial last checked for feasibility with SAP S/4HANA Cloud Release 2508

#### Our Example

A several tutorials spanning example will show extensibility along custom Bonus Management applications.

In the first parts a Manager wants to define business objects "Bonus Plan" for employees. A Bonus Plan is used to save employee specific rules for bonus entitlement.

### Open the UI to be adapted

Start typing **Bonus Plan** in the Launchpad search and open the App from the results.

![Bonus Plans application from search results](FLP_search_resultBonusPlans.png)

Press **Go** to get the list of all Bonus Plans. **Open** a bonus plan's detail view by clicking its list item.

![Open Bonus Plan's detail view](UI_openBoDetails.png)

This is the screen that will be adapted.

![Bonus Plan's detail view before adaptation](UI_BoDetailsBeforeAdaptation.png)

### Switch to Adaptation mode

1. Open User Profile via the corresponding application's menu action
2. Open the adaptation mode via **Adapt UI**.

![Open User Settings and Adapt UI](UI_userProfileAdaptUI.png)

>**Hint:** If you're using Key User Adaptation for the first time, a message may appear introducing the General UI Adaptation Tour. You can start the tour by selecting `Yes`, or choose `No` to take it later.

### Create a UI group

Editable UI elements can be recognized by getting a border when hovering over them.

![Editable UI element](UI_editableElement.png)

By right clicking onto them you get options to adapt the UI. As these options are partly type dependent you might need to find the right element first to get the option you need.

**Hover** over the **General Information** area until it gets the border and open the context menu via **Right Click**.

![Create UI Group](UI_createGroup.png)

**Create Group** and give it the title "Bonus Data".

### Move UI elements

Editable fields can simply be dragged and dropped as well. **Drag** the Validity Start Date field.

![Movable UI Element](UI_movableElement.png)

**Drop** it to the Bonus Data group.

![Drop dragged UI Element](UI_dropElement.png)

Repeat **Drag & Drop** into Bonus Data group for the fields:

- Validity End Date
- Target Amount
- Low Bonus Assignment Factor
- High Bonus Assignment Factor
- Low Bonus Percentage
- High Bonus Percentage
- Employee ID
- Employee Name

### Apply UI changes

Last you make the UI adaptations available to all users in the system.

Click the activation button (wand icon).

<!--border-->
![Activate UI changes](UI_activate.png)

In the opening pop up give a name for the new version and **Confirm** that.

![Give Version name during activation](UI_activateGiveVersionConfirm.png)

Click the Publishing button (truck icon) to make the last activated layout version available to all users in the system.

<!--border-->
![Publish UI version](UI_publish.png)

Finally exit adaptation mode (cross icon).

<!--border-->
![Exit UI Adaptation mode](UI_exit.png)

### Test yourself
