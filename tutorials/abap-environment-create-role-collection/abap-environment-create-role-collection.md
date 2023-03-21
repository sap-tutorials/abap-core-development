---
parser: v2
auto_validation: true
primary_tag: software-product>sap-btp--abap-environment
tags: [  tutorial>intermediate, programming-tool>abap-development, software-product>sap-business-technology-platform, tutorial>license ]
time: 45
author_name: Merve Temel
author_profile: https://github.com/mervey45
---

# Add Scopes and Create Role Collection Mapping
<!-- description --> Add scopes for filtering apps and create role collection mapping with SAP Cloud Platform ABAP environment.

## Prerequisites  
 - Create a developer user in a SAP Cloud Platform ABAP Environment system.
 - Download Eclipse Photon or Oxygen and install ABAP Development Tools (ADT). See <https://tools.hana.ondemand.com/#abap>.

## You will learn  
  - How to add scopes for filtering apps
  - How to create roles and role collections
  - How to create role collection mappings
  - How to assign users to role collections

---

### Add Scope for filtering apps


  1. Open SAP Web IDE and your project `ROOM_MTA_XXX` and add scope to `xs-security.json`.

    ```JSON
         {  

             "name": "$XSAPPNAME.Room-maintain",
             "description": "View data"

         }          
    ```


  2.  Add role template to xs-security.json

    ```JSON
          {
                "name": "RoomTemplate",
                "description": "Role for viewing data",
                "scope-references": ["$XSAPPNAME.Room-maintain"]
          }   
    ```

  3. Add required scope to `webapp/manifest.json`.

    ```JSON
          {
              "sap.platform.cf":
                                {		
                                  "oAuthScopes": ["$XSAPPNAME.Room-maintain"]
                                }
          }   
    ```


### Deploy UI to Cloud Foundry


  1. Right click on `ROOM_MTA_XXX` file and choose **Deploy** > **Deploy to SAP Cloud Platform**.

      ![Deploy UI to Cloud Foundry](Deploy.png)


  2.  Choose again the **API Endpoint**, **Organization** and **Space**. Now click on **Deploy**.

      ![Deploy UI to Cloud Foundry](Deploy1.png)


### Create role & role collection

  1. Switch to your SAP Cloud Platform Cockpit, select your `appRouter`, click **Roles** and **New Role**

      ![Create role & role collection](role.png)

  2. Create a new role.
     - Name: `MyRoomTemplate_XXX`
     - Template: `RoomTemplate`

     Click **Save**.

      ![Create role & role collection](role2.png)

  3. Select your subaccount, click **Role Collections** in the security area and click **New Role Collection**.

      ![Create role & role collection](role3.png)

  4. Create a new role collection.
     - Name: `MyCollection_XXX`

     Click **Save**.

      ![Create role & role collection](role4.png)


  5. Select your created role collection.

      ![Create role & role collection](role5.png)

  6. Add a role to your role collection in Cloud Foundry subaccount.

      ![Create role & role collection](role6.png)


### Create role collection mapping

  1. In your SAP Cloud Platform Cockpit, select your **Role Collection Mappings** and click **New Role Collection Mapping**

      ![Create role collection mapping](mapping.png)

    Hint: Groups attribute and assignment to business user needs to be provided in identity provider!

  2. Create a new role collection mapping.
     - Role collection: `MyCollection_XXX`
     - Value: `BR_ROOM_XXX`

     Click **Save**.

      ![Create role collection mapping](mapping2.png)



### Assign user to role collection

  1. Select your trust configuration in your SAP Cloud Platform Cockpit. Select **Role Collection Assignment**, enter your e-mail address, click **Show Assignments** and **Assign Role Collection**.

      ![Create role & role collection](collection.png)

  2. Select `RoomsRole` and click **Assign Role Collection**.

      ![Create role & role collection](collection2.png)

  3. Select your subaccount and your application route will be shown.

      ![Create role & role collection](collection3.png)

     Hint: `<route-on-cf> = <your_url>.sap.hana.ondemand.com`


### Test yourself



---
