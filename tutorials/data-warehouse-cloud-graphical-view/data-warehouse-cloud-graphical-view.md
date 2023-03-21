---
parser: v2
auto_validation: true
time: 15
tags: [ tutorial>beginner, products>sap-data-warehouse-cloud]
primary_tag: products>sap-data-warehouse-cloud
---

# Create a Graphical View Model in SAP Data Warehouse Cloud
<!-- description --> With SAP Data Warehouse Cloud, you can use a graphical interface to create data views. You can drag and drop sources from the Source Browser, join them as appropriate, add other operators to transform your data, and specify measures and other aspects of your output structure in the output node.

## Prerequisites
 - You have [imported your dataset into your Space.](data-warehouse-cloud-import-dataset-csv)

## You will learn
  - About graphical views
  - How to create a graphical view
  - How to rename columns in your graphical view
  - How to restore columns in your graphical view
  - How to deploy a graphical view

## Intro
Add additional information: Background information, longer prerequisites

---

### About Graphical views


In SAP Data Warehouse Cloud, you can use the graphical view builder to easily create data views.  This allows you to work intuitively without having to be familiar with SQL statements.

In the graphical view builder, you have many resources to model your data, combine them from many sources and assigning business semantics that make your output easier to understand. Here is an example of a graphical view modelled in SAP Data Warehouse Cloud.

![Example of Graphical View](Picture1.png)

i Note
If you are comfortable writing SQL code or want to use SQL Script to create your view, you can use the SQL View editor.


### Create a Graphical View


1.	Go to the Data Builder and click on the New Graphical View button.

    ![New Graphical View](Picture2.png)

2.	Now that you are in the graphical model builder, it's time to find the data. As you imported CSV files, your data is under Repository, on the top right-hand side of the screen.

    ![Repository](Picture3.png)

3.	To start building your model, click and drag the `SalesOrders` table onto the canvas.

    ![Graphical Interface](Picture4.png)

4.	As you can see, an output node appears on the canvas as soon as your drop your table in it. The output node is where all of our join table information will appear once you've completed the model.
5.	Click on the output node and then click on the data preview button to see a preview of the sales orders data.

    ![Data Preview](Picture5.png)

6.	Next, drag the table `SalesOrderItems` on top of the `SalesOrders` table to join the two tables. The icon that has appeared is our join node called Join One. The column `SalesOrderID` from both tables is automatically joined based on entity-relationship model.

    ![SalesOrderID Join](Picture6.png)

7.	Now drag in the `BusinessPartners` table. We'll go ahead and join it to the `SalesOrders` table. The column `Partner ID` from each table will join automatically, but if you want to double check, look at the join properties panel.

    ![PartnerID](Picture7.png)

8.	And now add the `BusinessPartners` table. Join it to the `SalesOrders` table. The column `PartnerID` from each table will join automatically.


9.	If an alert pops up on the Join 1 panel, connect the column `SalesOrderID` from both tables.

    ![SalesOrderID Join](Picture8.png)

10.	Next, join the `Addresses` table to your `BusinessPartners` table. These will join automatically based on the association in the entity relationship model.

    ![Addresses Table](Picture9.png)

11.	If an alert has popped up on the Join 2 node, click on the join and make sure the `PartnerID` column from both tables is connected.


Now you've joined all the tables for this mission.



### Rename and Restore Columns


With the graphical view in place, rename some columns to help others understand better what the data is about. Rename the `Grossamount` column from the `SalesOrders` table and from the `SalesOrderItems` table so you can tell them apart.

1.	To do this, click on the projection one node immediately on the right-side of the output node.

    ![Projection Node](Picture10.png)

2.	Search for the `GrossAmount` column in the projection properties panel.

    ![Search for Column](Picture11.png)

3.	If one of the columns is greyed out, this means the column has been automatically removed. Click on the dots next to it and select Restore column.

    ![Restore Column](Picture12.png)

4.	Next, click on the column name. On the canvas, you will see a highlight that shows you exactly where the column originates from.


5.	Rename the `GrossAmount` column originating from the `SalesOrderItems` table to `GrossAmount_items`.


6.	Then rename the `GrossAmount` column originating from the `SalesOrders` table to `GrossAmount_orders`.

    ![Rename Column](Picture13.png)


### Save and Deploy


You have successfully created your graphical view. It is now extremely important to first save, and then deploy your view. When you save an object, it is stored in the SAP Data Warehouse Cloud repository, which contains the design-time definitions of all your objects. When you deploy an object, you are creating a run-time version for use in the SAP Data Warehouse Cloud database.


Click on the save icon on the top left and give your graphical view an apt name.

![Save](Picture14.png)

Once done, click on the deploy icon next to the save icon to deploy your model.

![Deploy](Picture15.png)



---
