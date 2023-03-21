---
parser: v2
auto_validation: true
time: 10
tags: [ tutorial>beginner, products>sap-data-warehouse-cloud]
primary_tag: products>sap-data-warehouse-cloud
---

# Define Measures and add Business Information to a View in SAP Data Warehouse Cloud
<!-- description --> SAP Data Warehouse Cloud allows you to add business information to your data, making it easier for all users to make sense of the data. This is what we call the semantic layer. You can add business names to columns within tables, as well as semantic types to better identify the type of data in columns.

## Prerequisites
 - You have [graphically created your data model.](data-warehouse-cloud-graphical-view)

## You will learn
  - Overview of semantic information
  - How to define measures
  - How to add business information
  - How to change business names of measures and attributes
  - How to change the semantic type of measures and attributes
  - Preview your modelled dataset

## Intro
Add additional information: Background information, longer prerequisites

---

### Overview of Semantic Information


Semantic information is simply a layer of information that is focused on providing a "translation" in business language to help all users understand each aspect of the data. Sometimes datasets can be hard to read for users who are not familiar with the way the data is structured, using a lot of abbreviations or acronyms. The semantic layer of SAP Data Warehouse Cloud can help you solve that issue by adding new column names, as well as defining the appropriate Semantic Type information for each column.

The following is a list of the potential semantic usage that you can determine for your tables and views:

- **Relational Dataset** - [default] Contains columns with no specific analytical purpose.
- **Dimension** - Contains attributes with master data like a product list or store directory, and supporting hierarchies (see [Create a Dimension](https://help.sap.com/viewer/c8a54ee704e94e15926551293243fd1d/cloud/en-US/5aae0e95361a4a4c964e69c52eada87d.html)).
- **Analytical Dataset** - Contains one or more measures and attributes. This is the principal type of artefact used by analytical clients (see [Create an Analytical Dataset](https://help.sap.com/viewer/c8a54ee704e94e15926551293243fd1d/cloud/en-US/30089bd2aa754ab996a62cf5842ae60a.html)).
- **Text** - Contains attributes used to provide textual content in one or more languages (see [Create a Text Entity for Attribute Translation](https://help.sap.com/viewer/c8a54ee704e94e15926551293243fd1d/cloud/en-US/b25726df116b463e97435ba720e48ac9.html)).

Though tables and views can both be identified with semantic usage, only views have the property Expose for Consumption, which makes them available to SAP Analytics Cloud and other analytical clients.

To be consumable as a model in SAP Analytics Cloud, your entity must be a view with:
- Semantic Usage set to Analytical Dataset (see [Create an Analytical Dataset](https://help.sap.com/viewer/c8a54ee704e94e15926551293243fd1d/cloud/en-US/30089bd2aa754ab996a62cf5842ae60a.html)).
- Expose for Consumption enabled.
- At least one measure identified (see [Measures](https://help.sap.com/viewer/c8a54ee704e94e15926551293243fd1d/cloud/en-US/33f7f291538a44a293a89f6f1cf1fa81.html)).



### Define Measures


Now it's time to define measures in your data. If you need more information on what measures are, please see [this help guide](https://help.sap.com/viewer/c8a54ee704e94e15926551293243fd1d/cloud/en-US/33f7f291538a44a293a89f6f1cf1fa81.html?q=measures).

1.	Go to the Data Builder. You may need to select your appropriate space first.
2.	Click on the graphical model that you created earlier.

    ![Data Builder](Picture1.png)

3.	Change the semantic usage of this to analytical data set in the Model Properties tab on the right-side of the screen. This will allow you to change attributes into measures.

    ![Semantic Type](Picture2.png)


> **i** It is necessary to define some of our columns as measures in order to create charts and graphs in SAP Analytics Cloud.

4.	To convert an attribute into a measure, simply drag and drop the attribute into the measures area in the Model Properties tab. In this example, define `GrossAmount_Items` as a measure.

    ![Drag Measure](Picture3.png)

5.	Define `GrossAmount_Orders` as a measure as well.

    ![Drag Measure 2](Picture4.png)

>**i** The system will not allow you to define dimensions as measures.




### Add Business Information


Next, add some more business information about the data model.

1.	Open the Business Purpose panel under Attributes.
2.	Here, fill in the description and purpose of this model, as well as the business contact person, responsible team and relevant tags.

    ![Business Information](Picture5.png)


### Change Business Names for Measures and Attributes


You can also change the business names of measures and attributes. This is particularly useful if some of your data columns have names that wouldn't be recognisable to your colleagues.

1.	To do this, click on the pencil icon next to Attributes.

    ![Business Names Icon](Picture6.png)

2.	This takes you to an interface that allows you to type new names alongside the technical names. This doesn't change the name of our column in the data itself, it just adds a business name in addition to it. You also have the option to add spaces or change column names to lowercase.

    ![Business Names](Picture7.png)


### Change the Semantic Type of Measures and Attributes


Another part of adjusting your semantic layer information is the opportunity to change the semantic type of your column. Semantic types identify that a column contains a value, a quantity, a date, geospatial or textual information, or another kind of semantic information.

For example, next to a column, open the drop-down list and find Geolocation - Latitude.

![Semantic Type](Picture8.png)

This tells the system specifically what kind of data is in the column, and is very useful when performing data analysis.

>**i** To add semantic types, you must set the semantic usage of your view or table to Dimension or Analytical Dataset.



### Save and Deploy


Once you are done adding your semantic information and types, don't forget to first save, and then deploy your view.

1.	Click on the save icon on the top left corner of the screen.

    ![Save](Picture9.png)

2.	Then, click on the deploy icon next to the save icon to deploy your model.

    ![Deploy](Picture10.png)


### Preview your Data


You have now successfully created, saved and deployed your analytical dataset.

If you wish, you can preview your data model by clicking on the data preview icon next to your output node.

![Data Preview](Picture11.png)

The preview is limited to a maximum of 1,000 lines and, by default, to the first 20 columns in the table or view. You can also reorder, sort, or filter your columns according to your needs in the data preview section of the data builder.

Having successfully created a working data model, you can now help Best Run Bikes bring together information from different parts of their business to understand more about their sales. You can now use these data models to visualise your sales or transaction data, and subsequently draw insights and make better business decisions in your organisation.




---
