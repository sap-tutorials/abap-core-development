---
parser: v2
auto_validation: true
time: 10
tags: [ tutorial>intermediate, topic>big-data, topic>artificial-intelligence]
primary_tag: products>sap-data-intelligence
author: Dimitri Vorobiev
---

# Organize Data In ML Data Manager - Pam2
<!-- description --> Utilize functionalities of ML Data Manager to organize data

## Prerequisites
 - Completed tutorials for beginner

## You will learn
  - How to create workspaces in ML Data Manager
  - How to upload your data in ML Data Manager

---

### Create a new workspace in ML Data Manager


1. Click [here](https://raw.githubusercontent.com/SAPDocuments/Tutorials-Contribution/tutorials/dataintelligence-trial-v3-train-model-01/IoT_A.csv) to **download** the `.csv` file `IoT_A.csv` to your local machine.

2. Login as the **`system`** user on the **`default`** tenant and click on the tile of **ML Data Manager**.

    ![Launchpad](./dataintelligence-trial-v3-train-model-ml-data-manager-01.jpg)

3. On the launch page of ML Data Manager you will find the **Create** button in the upper right corner. Click on it and type **`pdms_trial`** in the pop-up window to create a new workspace.

    ![ML-Data-Manager-Launch-Page](./dataintelligence-trial-v3-train-model-ml-data-manager-02.jpg)

    ![ML-Data-Manager-pop-up-1](./dataintelligence-trial-v3-train-model-ml-data-manager-03.png)

4. Create a new `Data Collection` by clicking on the button **Create**. In the pop-up window type `IoT_data` as the name of the collection.

    ![ML-Data-Manager-data-collection](./dataintelligence-trial-v3-train-model-ml-data-manager-04.jpg)

5. Click on **Edit in Metadata Explorer** which will launch the `Metadata Explorer` application in a new browser tab.

    ![ML-Data-Manager-data-collection-content](./dataintelligence-trial-v3-train-model-ml-data-manager-05.jpg)


### Upload your data to the data lake

In the **Metadata Explorer** you can upload the `.csv` file `IoT_A.csv`(which you downloaded in Step 1) to this folder by clicking on the upload symbol.

![ML-Data-Manager-upload](./dataintelligence-trial-v3-train-model-ml-data-manager-06.jpg)

After uploading the data you can switch back to the browser tab of **ML Data Manager**, where you should now see the `IoT_A.csv` in the content tab. If not, please click on **refresh button** of your browser.

![ML-Data-Manager-upload-completed](./dataintelligence-trial-v3-train-model-ml-data-manager-07.jpg)



### Extract features

1. Click on the row with the title `IoT_A.csv`.  

2. Select **Features List** tab.

3. Click on the **Source File** icon.

    <!-- border -->![FeaturesList-ClickOnSource](dataintelligence-trial-v3-train-model-ml-data-manager-08.jpg)

4. In the pop up window below select the `IoT_A.csv` file.

    <!-- border -->![Select the source file](dataintelligence-trial-v3-train-model-ml-data-manager-09.jpg)

5. A preview of the dataset will appear where you can customize the data type of each column. In this case the default data types are correct, click on **Save** in the bottom right corner to save the feature list.

![Link text e.g., Destination screen](dataintelligence-trial-v3-train-model-ml-data-manager-10.jpg)

Great! Your dataset is now ready for use in the `ML Scenario` application which we will use in the next section of this tutorial.




---
