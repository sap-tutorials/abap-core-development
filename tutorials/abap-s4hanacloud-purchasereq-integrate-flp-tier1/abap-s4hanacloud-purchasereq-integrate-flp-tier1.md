---
parser: v2
auto_validation: true
primary_tag: software-product-function>sap-s-4hana-cloud--abap-environment
tags:  [ tutorial>beginner, programming-tool>abap-development, programming-tool>abap-extensibility]
time: 25
author_name: Merve Temel
author_profile: https://github.com/mervey45
--- 
 
# Add the Shopping Cart Fiori Application to FLP  
<!-- description --> Integrate your list report application into ABAP Fiori launchpad.

## Prerequisites  
- This tutorial can be used in both SAP S/4HANA Cloud, private edition system and SAP S/4HANA on-premise system with release 2023 FPS01. We suggest using a [Fully-Activated Appliance](https://blogs.sap.com/2018/12/12/sap-s4hana-fully-activated-appliance-create-your-sap-s4hana-1809-system-in-a-fraction-of-the-usual-setup-time/) in SAP Cloud Appliance Library for an easy start without the need for system setup.
- The steps for fully-qualified Domain Name & SSL Certificates can be found [here](https://www.sap.com/documents/2020/06/109b1be0-9e7d-0010-87a3-c30de2ffd8ff.html).
- For SAP S/4HANA on-premise, create developer user with full development authorization 
- Initial setup of the [Launchpad](https://help.sap.com/docs/ABAP_PLATFORM_NEW/a7b390faab1140c087b8926571e942b7/b52e74afea0c4aeb8b642b8e6ba8911f.html?version=202210.002)
- You user has assigned following [roles](https://help.sap.com/docs/ABAP_PLATFORM_NEW/a7b390faab1140c087b8926571e942b7/85be3fff35604fa09a1668dd97ef4407.html?locale=en-US&q=SAP_FLP_USER)
    - `SAP_FLP_USER`
    - `SAP_FLP_ADMIN`
  Find here the [documentation](https://help.sap.com/docs/ABAP_PLATFORM_NEW/a7b390faab1140c087b8926571e942b7/445570c460b24244bbacd962fe731326.html?version=202210.002).
- You finished [Create a SAP Fiori Shopping Cart App and Deploy it](abap-s4hanacloud-purchasereq-deploy-ui) tutorial.
- Pages and Space should be activated.

## You will learn  
- How to create a semantic object
- How to create a technical catalog 
- How to create SAP Fiori business catalog
- How to create a space and page 
- How to create a business role and assign to business user 
- How to run an app from FLP


## Intro
In this tutorial, wherever ### appears, use a number (e.g. 000). This tutorial is done with the placeholder 000.

---

### Create a semantic object

  1.  Open **SAP Logon** and log into your system. Execute following transaction to create a semantic object **`/ui2/semobj`**. You also need to add **`/n`** before your transaction.

      ![object](object.png)

  2. Click **Edit** and select the check mark.

      ![object](object2.png)

  3. Click **New Entries**.

      ![object](object3.png)

  4. Add your **semantic object**:
     - Semantic Object: **`z_shopping_cart_tier1_###`** 
     - Semantic Object Name: **`z_shopping_cart_tier1_###`** 
     - Semantic Object Description: Shopping Cart ###
     
      Click **Save**.
   
      ![object](newxx.png) 

  5. Select and confirm your transport request.
   
      ![object](save.png) 



### Create SAP Fiori business catalog 

Find here the documentation on [Best Practices for Managing Catalogs](https://help.sap.com/docs/SAP%20Fiori%20launchpad/d4650bf68a9f4f67a1fda673f09926a9/af35d42e7d4f49d7b8e46080cd01c299.html?version=753.04).
 
In this step, you will create a business catalog. The SAP Fiori business catalog is client-dependent, so it is part of the customizing. 

  1. Execute following transaction to create a business catalog **`/ui2/flpcm_cust`**. You also need to add **`/n`** before your transaction.

      ![item](item.png)

  2. Search for technical catalog **`SAP_TC_UI5_REPO_UIAD`** and press **Go** to check the existing content.  
   
      ![item](new6x.png)

  3. Now you can see the existing content of generic technical catalog `SAP_TC_UI5_REPO_UIAD`. During UI deployment automatically an app descriptor item was created and assigned to this technical catalog. Click **Create** to create a new business catalog.

      ![item](new7x.png)
 
  4. Create a **new catalog**:
     - New ID: **`z_bc_shopcart_###`**
     - New Title: Business Catalog - Shopping Cart ###

      ![item](new8x.png)

      Select your transport request and description. Select the check mark.

    > **Hint**: The business catalog has the naming convention `BC`.

  5. Click **Add Tiles/Target Mappings** and select **`Add Tiles/TMs to Selected Catalog`**.

      ![item](new9x.png)

  6. Search for **`z_shopping_cart_tier1_###`**, press **Go**, select your entry and click **Add Tile/TM Reference**.
   
      ![item](new10x.png) 

  7. Check your result.
   
      ![item](new11x.png)


### Create a business role and assign business catalog and business user


  1. Execute following transaction to create a business role and assign it to your user **`pfcg`**. 

      ![user](user.png)   

  2. Enter **`Z_BR_SHOPCART_###`** as Role and click **Single Role**.

      ![user](newx2.png)

  3. Enter a description and click **Save**.

      ![user](newx3.png)

  4. Navigate to **Menu** and select **Transaction** > **SAP Fiori Launchpad** > **Launchpad Catalog**.

      ![user](new14.png)

  5. Enter **`Z_BC_SHOPCART_###`** as Catalog ID, select the check mark and click save.

      ![user](new15.png)

  6. Navigate to **Authorization** and select the edit icon for **Change Authorization Data**.

      ![user](new16.png)

  7. Click the icon.

      ![user](newx4.png)

  8. Select the check mark.
   
      ![user](new18.png)

  9. Go back.

      ![user](new19.png)

 1.  Navigate to **User**, enter your user, click **Save** and click **User Comparison**.

      ![user](usercomparison.png)


### Run application via app finder

  1. Execute following transaction to run an app from SAP Fiori Launchpad **`/ui2/flp`**. You also need to add **`/n`** before your transaction.

     ![flp](flp.png)
    
  2. Log in with the user you assigned the business role `Z_BR_SHOPCART_###` to.

     ![flp](login.png)

  3. Click your user on the top right corner and select **App Finder**.

     ![flp](appfinder.png)

  4. Search for **Shopping Cart ###** and press the + symbol.

     ![flp](new21.png)

  5. Select **My Home** to add your application to the my home page.

     ![flp](new22.png)

  6. Go to your home page and now you can see your application. Click your application and check your result.
   
     ![flp](new23.png)



### Create a space and page 
 
  1. Execute following transaction to create spaces and pages **`/ui2/flp`**. You also need to add **`/n`** before your transaction.

      ![space](space.png)   

  2. Search **Manage Launchpad Spaces** and select it.
   
      ![space](space3x.png)

  3. Click **Create**.
 
      ![space](space4.png) 

  4. Create a **new space and page**:
     - Space ID: **`z_shopping_cart_tier1_###`**
     - Space Description: Space for Shopping Cart ### 
     - Space Title: Shopping Cart ### 
     - Check mark **Also create a page**
     - Page ID: **`z_shopping_cart_tier1_###`**
     - Page Description: Page for Shopping Cart ### 
     - Page Title: Shopping Cart ### 

      ![space](new24x.png)

      Select your transport request and click **Create** and assign your transport request.

  5. Click **Save**.
   
      ![space](newx25x.png)

  6. Navigate to your page `z_shopping_cart_tier1_###`.

      ![space](new28x.png)

  7. Inside your page **`z_shopping_cart_tier1_###`** click **Edit** to add your page content.
   
      ![space](new29x.png)

 
  8. Select the menu and click **Select Catalogs**.
   
      ![space](new30x.png)

  9. Search for your catalog **`z_bc_shopcart_###`**, select it and click **Select**.

      ![space](new31x.png)

 10. Select your business catalog **`z_bc_shopcart_###`** and click **Add**.

      ![space](new32x.png)

 11. Enter the section title: **Shopping Cart Section** and click **Save**.

      ![space](new33x.png)



### Add space to business role

  1. Execute following transaction **`pfcg`** to open your business role and add the newly created space and page to it.

      ![user](user.png)   

  2. Enter **`Z_BR_SHOPCART_###`** as Role and click the edit symbol.

      ![user](new34.png)

  3. Go back and navigate to **Menu** again. Select the **arrow** > **SAP Fiori Launchpad** > **Launchpad Space**. 

      ![user](new35.png)

  4. Enter **`Z_SHOPPING_CART_TIER1_###`** as Space ID and select the check mark.
   
      ![user](new36x.png)

  5. Add a description and click **Save**.
   
      ![user](new37x.png)


 
### Run an app from FLP


  1. Execute following transaction to run an app from SAP Fiori Launchpad **`/ui2/flp`**. You also need to add **`/n`** before your transaction.

     ![flp](flp.png)
    
  2. Navigate to **Shopping Cart ###** and select your tile to check your result.

     ![flp](new38.png)


### Test yourself


### More information

Find here the documentation on [SAP Fiori Launchpad](https://help.sap.com/docs/SAP_FIORI_LAUNCHPAD).