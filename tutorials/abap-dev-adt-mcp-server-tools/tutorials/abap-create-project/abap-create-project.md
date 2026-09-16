---
parser: v2
auto_validation: true
primary_tag: topic>abap-development
tags: [  tutorial>beginner, software-product>sap-netweaver ]
time: 15
author_name: Julie Plummer
author_profile: https://github.com/julieplummer20

---

# Create an ABAP Project in ABAP Development Tools (ADT)
<!-- description --> Configure the Eclipse IDE with the ABAP Development Tools for SAP NetWeaver (ADT) and create an ABAP project.

## Prerequisites

 - You have a valid instance of an on-premise AS ABAP server, version 7.51 or higher (some ABAP Development Tools may not be available in earlier versions). 
 - **Tutorial**:  [Download the Eclipse IDE and add the ABAP Development Tools (ADT) Plugin | SAP Tutorials](https://developers.sap.com/tutorials/abap-install-adt.html)

    > You can download a free ABAP application server, developer edition, from:
    - [dockerhub: ABAP Platform Trial ](https://hub.docker.com/r/sapse/abap-platform-trial/tags)
    - [SAP Developers Community - Trials and Downloads](https://developers.sap.com/trials-downloads.html?search=AS%20ABAP), e.g. AS ABAP 7.52 SP04


## You will learn

- How to configure your ABAP development environment with ABAP Development Tools (ADT)
- How to create your first ABAP project

---


### Select the ABAP perspective

1. Open ABAP Development Tools (ADT). If the Welcome Page appears, close it.

2. Switch to the ABAP perspective choosing **Open Perspective** from the toolbar or using the menu: **Window > Open Perspective > Other**.

    ![Image depicting step2-open-perspective](step2-open-perspective.png)

3. Then switch to the ABAP Perspective by choosing **ABAP** in the list and choosing **OK**.

    <!-- border -->!
    ![Image depicting step2b-abap-perspective](step2b-abap-perspective.png)


### Familiarize yourself with the tooling layout

In the ABAP Perspective, the ABAP tools are ideally positioned in the IDE to facilitate your development tasks. Nevertheless you are free to rearrange all views and editors to your personal needs.

<!-- border -->!
![Image depicting step3-tooling-layout](step3-tooling-layout.png)


### Create a new ABAP project

You need to create one ABAP project for each system connection. To do this:

1. In the **File** menu, choose **New > Other ABAP Project**. Choose the ABAP backend system from the list of SAP System connections, then choose **Next**. For example, if you are using the AS ABAP 7.5x developer edition, this will be **`NPL`** or **`A4H`**.

    ![Image depicting step4-system-connection](step4-system-connection.png)

2. In Connection Settings, accept the defaults and choose **Next**.

    <!-- border -->!
    ![Image depicting step4b-connection-settings](step4b-connection-settings.png)

3. Enter your logon data and choose **Finish**.

    <!-- border -->!
    ![Image depicting step4c-logon-data](step4c-logon-data.png)

The ABAP Project has been created. It represents a system connection to your chosen SAP system.

![Image depicting step4d-project-created](step4d-project-created.png)


### Test yourself



