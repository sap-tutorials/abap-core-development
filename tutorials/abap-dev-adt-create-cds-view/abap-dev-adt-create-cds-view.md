---
parser: v2
auto_validation: true
primary_tag: programming-tool>abap-development
tags: [  tutorial>beginner, software-product>sap-netweaver ]
time: 20

---
# Create a Simple ABAP CDS View in ADT
<!-- description --> You will learn how to create a CDS (Core Data Services) view using ABAP Development Tools (ADT).

## Prerequisites
- You have a valid instance of an on-premise AS ABAP server, version 7.51 or higher (some ABAP Development Tools may not be available in earlier versions)
- You have run the transaction `SEPM_DG_OIA_NEW` or transaction `STC01 -> tasklist SAP_BASIS_EPM_OIA_CONFIG`. (If you do not, your CDS view will display empty.)
-	**Tutorial**: [Create an ABAP Project in ABAP Development Tools (ADT)](abap-create-project)
- **Tutorial**: [Create an ABAP Package](abap-dev-create-package)

## You will learn  
- How to use the new Core Data Services (CDS) tools in ABAP Development Tools for Eclipse (ADT).
- How to use the following ABAP and SQL elements in a CDS view:
    - SELECT statement
    - CASE statement
    - WHERE clause

## Intro

> In this tutorial, you will create an ABAP Dictionary-based CDS view. As of ABAP AS 7.57, such views are deprecated. This tutorial is available for compatibility purposes only.
> For an short, up-to-date tutorial on CDS View Entities, see:
> **Tutorial**:[Create an ABAP Core Data Services (CDS) View in ABAP On-Premise](abap-dev-create-cds-view)

CDS is an extension of the ABAP Dictionary that allows you to define semantically rich data models in the database and to use these data models in your ABAP programs. CDS is a central part of enabling code push-down in ABAP applications.

You can find more information about these deprecated CDS Views here:
- [ABAP keyword documentation, version 7.51: CDS Views](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abencds.htm)

You can find more information about CDS View Entities here:
- [https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abencds_v2_views.htm](https://help.sap.com/doc/abapdocu_latest_index_htm/latest/en-US/index.htm?file=abencds.htm)
- [SAP Community](https://community.sap.com/topics/abap).
- 
Throughout this tutorial, objects name include the suffix `XXX`. Always replace this with your group number or initials.

---


### Create a CDS view

  1. In the context menu of your package choose **New** and then choose **Other ABAP Repository Object**.

  <!-- border -->
  ![Image depicting step1-newObject](step1-newObject.png)

  2. Select **Data Definition**, then choose **Next**.
    
  <!-- border -->
  ![Image depicting step2-DataDef](step2-DataDef.png)

  3. Enter the following values, then choose **Next**:
   
  -	Name = **`Z_INVOICE_ITEMS_XXX`**
  - Description = **Invoice Items**
  - Referenced Object: **`sepm_sddl_so_invoice_item`**

    <!-- border -->
    ![Image depicting step3-enterValues](step3-enterValues.png)

  4. Accept the default transport request (local) by simply choosing **Next** again.

  5. Select the entry **Define View**, then choose **Finish**

  <!-- border -->
  ![Image depicting step5-defineView](step5-defineView.png)


### Enter the data source

The new view appears in an editor, with the fields from the referenced object, `sepm_sddl_so_invoice_item`. In this editor, enter the following values:

1. Enter `Z_ITEMS_XXX` as the SQL view name.

    Your CDS view should now look like this:

    <!-- border -->
      ![step3a-new-cds-view](step3a-new-cds-view.png)

    > The SQL view name is the internal/technical name of the view which will be created in the database. `Z_Invoice_Items` is the name of the CDS view which provides enhanced view-building capabilities in ABAP. You should always use the CDS view name in your ABAP applications.

2. Delete all the fields except:

  ```ABAP
  key sales_order_invoice_item_key,
      currency_code,
      gross_amount
  ```


### Use an existing CDS association

You will now model the relationships between data sources by using some existing CDS associations. You can use associations in path expressions to access elements (fields and associations) in related data sources without specifying JOIN conditions. You can now display the element info by positioning the cursor on the data source name **`sepm_sddl_so_invoice_item`** and choosing **F2**.

To see the related data sources that can be accessed using associations, scroll down.
To see details about the target data source of the association header, choose the hyperlink **`sepm_sddl_so_invoice_header`**.

<!-- border -->
![Image depicting step8-CdsAssociations](step8-CdsAssociations.png)


### Add fields from existing associations

You will now add fields of related data sources to the SELECT list of `Z_Invoice_Items`, using the associations in path expressions. Each element in the path expression must be separated by a period.

1. Add the association **`header`** to your selection list, preferably with a comment, by adding the following the code. do not forget to add a comma after the previous item, `gross_amount`:

    ```ABAP
    //      * Associations *//
            header    
    
    ```

    <!-- border -->
    ![Image depicting step5-association](step5-association.png)

2. You will get an error, "Field header must be included in the selection list together with field `SEPM_SDDL_SO_INVOICE_ITEM.SALES_ORDER_INVOICE_KEY`". Resolve this by adding the field **`sepm_sddl_so_invoice_item.sales_order_invoice_key`** to the Select statement.

3. Add the `company_name` of the business partner to the SELECT list using the associations **header** and **buyer** in a path expression

4.	Add the `payment_status` from the invoice header to the SELECT list using the association **header**

    ```ABAP
    header.payment_status
    ```

    <!-- border -->
    ![Image depicting step9-AddRelatedFields](step9-AddRelatedFields.png)


### Add a CASE statement

If the invoice has been paid, you want to set the **`payment_status`** to X (true). Do this by implementing a CASE expression, assigning the alias `payment_status` to the CASE expression.

Remove the existing declaration, **`header.payment_status,`** and replace it with the following code. Do not forget to separate the new, calculated field **`paid`** and the association **`header`** with a comma.


```ABAP
case header.payment_status
    when 'P' then 'X'
    else ' '
end as paid,

```

<!-- border -->
![Image depicting step5a-case-paid](step5a-case-paid.png)

> You can check your code below.


### Add a WHERE clause

You will now filter the results so that only invoice items with `currency_code = 'EUR'` are retrieved.

1. Add a WHERE clause:

    ```ABAP

    WHERE currency_code = 'EUR'

    ```

    <!-- border -->
    ![step6a-where-eur](step6a-where-eur.png) 

2. Save and activate the data definition by choosing **Save** (`Ctrl+S`) and **Activate** (`Ctrl+F3`).

    <!-- border -->
    ![Image depicting step14-saveAndActivate](step14-saveAndActivate.png)


### Check your code and view your changes

Your CDS view code should look something like this:

```ABAP
@AbapCatalog.sqlViewName: 'Z_ITEMS_XXX'
@AbapCatalog.compiler.compareFilter: true
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Invoice Items'

define view Z_INVOICE_ITEMS_XXX
  as select from sepm_sddl_so_invoice_item
{

  key sales_order_invoice_item_key,
      sepm_sddl_so_invoice_item.sales_order_invoice_key,

      header.buyer.company_name,

      currency_code,
      gross_amount,

      case header.payment_status
          when 'P' then 'X'
          else ' '
      end as paid,

      //      * Associations *//
      header
}

where
  currency_code = 'EUR'

```
Open the CDS View in the Data Preview by choosing **F8**. Your CDS View should look roughly like this:

  <!-- border -->
  ![Image depicting step15-data-preview](step15-data-preview.png)


### Test yourself



