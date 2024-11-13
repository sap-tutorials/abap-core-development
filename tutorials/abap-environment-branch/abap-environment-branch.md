---
parser: v2
auto_validation: true
primary_tag: software-product>sap-btp--abap-environment
tags: [  tutorial>beginner, programming-tool>abap-development, software-product>sap-business-technology-platform, tutorial>license ]
time: 20
---

# Transport ABAP Objects Using git-based Change and Transport System (gCTS)
<!-- description --> Transport ABAP development objects using gCTS and the Manage Software Components app. Each software component has a 1:1 relationship with a git repo.

## Prerequisites  
  - SAP BTP, ABAP environment user
  - ADT version 2.96 or higher
  - Administrator role assigned to user

## You will learn
  - How to create software component
  - How to pull software component
  - How to release transport request
  - How to create main branch
  - How to create other branches

## Intro
In this tutorial, wherever `XXX` appears, use a number (e.g. `000`).

---


### Create software component

1. Logon to ADT, right-click on your ABAP cloud project and select **Properties**.

    ![Create software component](component.png)

2. Click on your system URL.

    ![Create software component](component2.png)

3. Logon to your ABAP system in your SAP Fiori launchpad and select **Manage Software Component**.

    ![Create software component](component3.png)

4. Select **`+`** to add a new software component.

    ![Create software component](component4.png)

5. Create a new software component:
      - Name: **`Z_SWCT_XXX`**
      - Type: **Development**

    Click **Save**.

    ![Create software component](component5.png)



### Pull software component

1. In your available software component **`Z_SWCT_XXX`** click **Pull**.

    ![Pull software component](pull.png)

    Note:  In the initial system, the 'main' branch will not be visible until you've released a transport request. If you are pulling in the production system, all branches will be available before you've released an ABAP transport request.

2. Click **OK**.

    ![Pull software component](pull2.png)

3. Check your result. Your pull is successful.

    ![Pull software component](pull3.png)



### Create ABAP class

  1. Switch to your ADT, right-click on **Favorite Packages** and select **Add Package**.

      ![Create ABAP class](class.png)

  2. Search for **`Z_SWCT_XXX`**, select it and click **OK**.

      ![Create ABAP class](class2.png)

  3. Right-click on your `superpackage` **`Z_SWCT_XXX`** and select **Add Package**.

      ![Create ABAP class](class3.png)

  4.  Create your **package:**
     - Name: **`Z_PCK_XXX`**
     - Description: **`Package XXX`**

      Click **Next>**.

      ![Create ABAP class](class4.png)

  5. Click **Next >**.

      ![Create ABAP class](class5.png)

  6. Select **Create a new request**:
     - Request Description: **`TR123456`**

      Click **Finish**.

    ![Create ABAP class](class6.png)

  7. Right-click on your package **`Z_PCK_XXX`** and select **ABAP Class**.

      ![Create ABAP class](class7.png)

  8.  Create your **ABAP class:**
     - Name: **`Z_CL_XXX`**
     - Description: **`Class XXX`**

     Click **Next>**.

      ![Create ABAP class](class8.png)

  9. Click **Finish**.

      ![Create ABAP class](class9.png)

  10. Replace your code with following:

    ```ABAP
    class z_cl_xxx definition
    public
    final
    create public .

    public section.
    interfaces if_oo_adt_classrun.
    protected section.
    private section.
    ENDCLASS.

    CLASS z_cl_xxx IMPLEMENTATION.
    METHOD IF_OO_ADT_CLASSRUN~MAIN.
    out->write('Hello world!').
    ENDMETHOD.
    ENDCLASS.
    ```

      **Save** and **activate**.



### Release transport request

  1. Select your class **`Z_CL_XXX`** and select **Transport Organizer** in your menu. Right-click on your transport task and select **Release**

      ![Release transport request](transport.png)

  2. Check your result and click **OK**.

      ![Release transport request](transport2.png)

  3. Right-click on your transport request and select **Release**.

      ![Release transport request](transport3.png)

  4. Check your result and click **OK**.

      ![Release transport request](transport4.png)



### Create new branch

  1. Switch to your ABAP system in your SAP Fiori launchpad, open your software component **`Z_SWCT_XXX`** in **Manage Software Component**.

      ![Create new branch](branch5.png)

  2. Refresh your page by pressing **`F5`**.

      ![Create new branch](branch.png)

  3. Select your main branch and click **`+`** to create a new branch.

      ![Create new branch](branch2.png)

  4. Create new branch:
     - Branch Name: **`branch_xxx`**

      Click **Create**.

    ![Create new branch](branch3.png)

  5. Check your result. Now your new branch **`branch_xxx`** is created.

      ![Create new branch](branch4.png)



### Test yourself


### More Information

- [Git-Enabled Change and Transport System (gCTS) | SAP Help](https://help.sap.com/docs/ABAP_PLATFORM_NEW/4a368c163b08418890a406d413933ba7/f319b168e87e42149e25e13c08d002b9.html)



