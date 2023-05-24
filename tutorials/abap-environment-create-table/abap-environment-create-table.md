---
parser: v2
auto_validation: true
primary_tag: software-product>sap-btp--abap-environment
tags: [  tutorial>beginner, programming-tool>abap-development, software-product>sap-business-technology-platform ]
time: 10
author_name: Merve Temel
author_profile: https://github.com/mervey45
---

# Create a Simple Database Table for ABAP Environment
<!-- description --> Create a database table in SAP BTP, ABAP Environment and pre-fill it with data.

## Prerequisites  
- You need an SAP BTP, ABAP environment [trial user](abap-environment-trial-onboarding) or license.

## You will learn
- How to create a database table
- How to `prefill` your database table with data

## Intro
<<<<<<< HEAD
In this tutorial, wherever `XXX` appears, use a number (e.g. `000`).

---

### Open Eclipse

Open Eclipse, and select **New** > **ABAP Package**.

![Open Eclipse](package.png)


### Create ABAP package

  1. Maintain following information in the appearing dialog and  click **Next**.

      - Name: **`Z_Booking_XXX`**
      - Description: **Package Booking**

      ![Create ABAP package](package2.png)

  2. Move on with **Next**.

      ![Create ABAP package](package3.png)

  3. Select transport request and click **Finish**.
=======
In this tutorial, wherever `###` appears, use a number (e.g. `000`).

---

### Create package

In this tutorial 000 is used as a group ID instead of ###.

  1. Open Eclipse, right-click on `ZLOCAL` and select **New** > **ABAP Package**.

      ![Open Eclipse](package.png)

  2. Maintain following information in the appearing dialog and  click **Next >**.

      - Name: **`Z_Booking_###`**
      - Description: **Package Booking ###**
      - `Superpackage:` `ZLOCAL`
      - Add to favorite packages: checked
      - Package Type: development

      ![Create ABAP package](package2.png) 

  3. Click **Next >**.

      ![Create ABAP package](package3.png)

  4. Create a new request and click **Finish**.
>>>>>>> 59f95048a11e62962d5c8eb49e89b6f027533a25

      ![Create ABAP package](package4.png)


<<<<<<< HEAD
### Open ABAP repository object

Right-click on your package and navigate to **New** > **Other ABAP Repository Object** from the appearing context menu.

![Open ABAP repository object](object.png)


### Create database table

  1. Search for **database table**, select the appropriate entry and click **Next**.

      ![Create database table](db.png)
  2. Maintain the required information and click **Next**.

      - Name: **`ZTBOOKING_XXX`**
      - Description: **Table Booking**

      ![Create database table](db2.png)

  3. On the next dialog, provide a transport request and click **Finish**.

      ![Create database table](db3.png)

  4. Check result. An empty table is now created.

      ![Check code](empty.png)


### Define database table

  1. Define the table columns (client, booking, `customername`, `numberofpassengers`, …). Specify client and booking as key fields, and the field `currencycode` as currency key for cost as displayed below. The table annotations (beginning with @) remain unchanged. For that, you can copy the database table definition provided below.

    ```ABAP
    @EndUserText.label : 'Demo: Booking Data'
    @AbapCatalog.enhancementCategory : #NOT_EXTENSIBLE
    @AbapCatalog.tableCategory : #TRANSPARENT
    @AbapCatalog.deliveryClass : #A
    @AbapCatalog.dataMaintenance : #LIMITED
    define table ztbooking_xxx {
    key client         : abap.clnt not null;
    key booking        : abap.int4 not null;
    customername       : abap.char(50);
    numberofpassengers : abap.int2;
    emailaddress       : abap.char(50);
    country            : abap.char(50);
    dateofbooking      : timestampl;
    dateoftravel       : timestampl;
    @Semantics.amount.currencyCode : 'ztbooking_xxx.currencycode'
    cost               : abap.curr(15,2);
    currencycode       : abap.cuky;
    lastchangedat      : timestampl;
    }
    ```

  2. Save and activate the database table.
=======
### Create database table

  1. Right-click on your package and navigate to **New** > **Other ABAP Repository Object** from the appearing context menu.

      ![Open ABAP repository object](table.png)

  2. Search for **database table**, select the appropriate entry and click **Next >**.

      ![Create database table](table2.png)

  3. Maintain the required information and click **Next >**.

      - Name: **`ZABOOKING_###`**
      - Description: **Database table for booking ###**

      ![Create database table](table3.png)

  5. Click **Finish**.

      ![Create database table](table4.png)

  6. Check result. An empty table is now created.

      ![Check code](table5.png)


  6. Define the table columns (client, booking, `customername`, `numberofpassengers`, …). Specify client and booking as key fields, and the field `currencycode` as currency key for cost as displayed below. The table annotations (beginning with @) remain unchanged. For that, you can copy the database table definition provided below.

    ```ABAP
    @EndUserText.label : 'Database table for booking ###'
    @AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
    @AbapCatalog.tableCategory : #TRANSPARENT
    @AbapCatalog.deliveryClass : #A
    @AbapCatalog.dataMaintenance : #RESTRICTED
    define table zabooking_### {

      key client         : abap.clnt not null;
      key booking        : abap.int4 not null;
      customername       : abap.char(50);
      numberofpassengers : abap.int2;
      emailaddress       : abap.char(50);
      country            : abap.char(50);
      dateofbooking      : timestampl;
      dateoftravel       : timestampl;
      @Semantics.amount.currencyCode : 'zabooking_###.currencycode'
      cost               : abap.curr(15,2);
      currencycode       : abap.cuky;
      lastchangedat      : timestampl;

    }
    ```

  7. Save and activate the database table.
>>>>>>> 59f95048a11e62962d5c8eb49e89b6f027533a25

      ![Define database table](saveandactivate.png)


### Create ABAP class

  1. Create a class in order to `prefill` our created database table. Right-click on your package and navigate to **New** > **ABAP Class** in the appearing context menu.

      ![Create ABAP class](class.png)

<<<<<<< HEAD
  2. Provide the required information and click **Next**.

    - Name: **`ZBP_GENERATE_BOOKINGS_XXX`**
    - Description: **Class to generate bookings**

      ![Create ABAP class](classnew.png)

  3. Provide a transport request and click **Finish**.

      ![Create ABAP class](classnew2.png)


### Replace source code

  1. Replace the source code of your class with the one provided below:

    ```ABAP
    CLASS zbp_generate_bookings_xxx DEFINITION
=======
  2. Provide the required information and click **Next >**.

      - Name: **`ZBP_GENERATE_BOOKINGSTP_###`**
      - Description: **Class to generate bookings**

      ![Create ABAP class](class2.png)

  3. Click **Finish**.

      ![Create ABAP class](class3.png)

  4. Replace the source code of your class with the one provided below:

    ```ABAP
    CLASS zbp_generate_bookingstp_### DEFINITION
>>>>>>> 59f95048a11e62962d5c8eb49e89b6f027533a25
      PUBLIC
      FINAL
      CREATE PUBLIC .

      PUBLIC SECTION.
        INTERFACES if_oo_adt_classrun.
      PROTECTED SECTION.
      PRIVATE SECTION.
    ENDCLASS.


<<<<<<< HEAD
    CLASS zbp_generate_bookings_xxx IMPLEMENTATION.

      METHOD if_oo_adt_classrun~main.
        DATA:it_bookings TYPE TABLE OF ztbooking_xxx.
=======
    CLASS zbp_generate_bookingstp_### IMPLEMENTATION.

      METHOD if_oo_adt_classrun~main.
        DATA:it_bookings TYPE TABLE OF zabooking_###.
>>>>>>> 59f95048a11e62962d5c8eb49e89b6f027533a25

    *    read current timestamp
        GET TIME STAMP FIELD DATA(zv_tsl).
    *   fill internal table (itab)
        it_bookings = VALUE #(
            ( booking  = '1' customername = 'Buchholm' numberofpassengers = '3' emailaddress = 'tester1@flight.example.com'
              country = 'Germany' dateofbooking ='20180213125959' dateoftravel ='20180213125959' cost = '546' currencycode = 'EUR' lastchangedat = zv_tsl )
            ( booking  = '2' customername = 'Jeremias' numberofpassengers = '1' emailaddress = 'tester2@flight.example.com'
              country = 'USA' dateofbooking ='20180313125959' dateoftravel ='20180313125959' cost = '1373' currencycode = 'USD' lastchangedat = zv_tsl )
<<<<<<< HEAD
         ).

    *   Delete the possible entries in the database table - in case it was already filled
        DELETE FROM ztbooking_xxx.
    *   insert the new table entries
        INSERT ztbooking_xxx FROM TABLE @it_bookings.

    *   check the result
        SELECT * FROM ztbooking_xxx INTO TABLE @it_bookings.
=======
        ).

    *   Delete the possible entries in the database table - in case it was already filled
        DELETE FROM zabooking_###.
    *   insert the new table entries
        INSERT zabooking_### FROM TABLE @it_bookings.

    *   check the result
        SELECT * FROM zabooking_### INTO TABLE @it_bookings.
>>>>>>> 59f95048a11e62962d5c8eb49e89b6f027533a25
        out->write( sy-dbcnt ).
        out->write( 'data inserted successfully!' ).

      ENDMETHOD.

    ENDCLASS.
    ```

<<<<<<< HEAD
  2. Save and active your class.
=======
  5. Save and active your class.
>>>>>>> 59f95048a11e62962d5c8eb49e89b6f027533a25

      ![Replace source code](saveandactivate.png)


### Run ABAP application

  1. Run your class as an ABAP application (console) or press **F9**.

<<<<<<< HEAD
      ![Run ABAP application](application.png)

  2. Check console output.

      ![Check console output](output.png)

  3. Switch back to your data definition and press **F8** to see the inserted data.

      ![Check inserted data](data.png)

  4. Now check your result.

      ![Check inserted data](result.png)
=======
      ![Run ABAP application](class5.png)

  2. Check console output.

      ![Check console output](result.png)

  3. Switch back to your data definition and press **F8** to see the inserted data.

      ![Check inserted data](result2.png)

  4. Now check your result.

      ![Check inserted data](result3.png)
>>>>>>> 59f95048a11e62962d5c8eb49e89b6f027533a25


### Test yourself
