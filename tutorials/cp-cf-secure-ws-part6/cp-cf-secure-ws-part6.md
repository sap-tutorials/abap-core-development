---
parser: v2
primary_tag: products>sap-cloud-platform
tags: [  tutorial>beginner ]
---

# Authorization - Static Checks and Configuration
<!-- description --> Set up authentication for your application to only grant access to authorized users

## Prerequisites  
 - **Proficiency:** Beginner 

## You will learn  
about roles and how to configure them for the end users so they can execute some (or all) operations.
## Time to Complete
**15 Min**

---

### Switch branch


1. Go to GitHub desktop, select the change to `myApp\server.js` and choose **Discard changes...**.
![Discard Changes](discard-changes.png)  
2. Under **Current branch**, select `5_Secure_Application_Authentication_Static_Check`.


### Clean up and redo the security-related preparation


1. To apply the changes in the `security/xs-security.json` file, execute the following commands.
```
cf unbind-service sapcpcfhw sapcpcfhw-uaa
```
```
cf unbind-service sapcpcfhwapr sapcpcfhw-uaa
```
```
cf delete-service sapcpcfhw-uaa -f
```
```
cf create-service xsuaa application sapcpcfhw-uaa -c security/xs-security.json
```

    > If you renamed all the occurrences of `sapcpcfhw` in the `manifest.yml` to some unique strings, as indicated in a previous tutorial, make sure that you replace the application names also in the commands above (that is, the second parameters `sapcpcfhw` and `sapcpcfhwapr` in the first two commands, respectively) with the unique names you defined in the `manifest.yml`.

2. Push the application again using the following command:
```
cf push
```

### Initial test


1. Open the application at: `https://<URL for your app>/users`.
   This still returns the message `401 Unauthorized`.
2. Open `https://<URL for the app router>/hw/users`.
   This returns the message `403 Forbidden`.

    > The reason for this behavior is that the application router only forwards calls to the application that come from users who are authorized to send calls. For more information, see the `appRouter/xs-app.json` and `security/xs-security.json` files.


### Configure permissions


1. Log on to the [SAP Cloud Platform cockpit](https://account.hanatrial.ondemand.com/#/home/welcome) and choose **`<own Trial account>`** | **trial Subaccount** | **Security** | **Role Collections**.
2. Create the following role collections: **`ViewerRC`** and **`ManagerRC`**.
  <ol type="a"><li>Choose **New Role Collection**.
  </li><li>In the **Name** field, enter `ViewerRC` and choose **Save**.
  </li><li>Create the `ManagerRC` in the same way.</li></ol>
3. Add the **Viewer** role of your application to the **`ViewerRC`** collection:
  <ol type="a"><li>Click **`ViewerRC`**.
  </li><li>Choose **Add Role**.
  </li><li>Select the appropriate values from the dropdowns for each field:
    - **Application Identifier**: Select the generated value for your app.
    - **Role Template**: Select Viewer.
    - **Role**: Select Viewer.

    </li><li>Choose **Save**.
  </li><li>Go back to the **Role Collections** page.
  </li><li>Click **`ManagerRC`** and proceed in the same way to add the **Manager** role to the **`ManagerRC`** collection.</li></ol>
4. In the [SAP Cloud Platform cockpit](https://account.hanatrial.ondemand.com/#/home/welcome), choose **`<own Trial account>`** | **trial Subaccount** | **Security** | **Trust configuration** | **SAP ID Service**.
5. Enter your e-mail address in the **User** field.
6. **Show Assignments** displays no data. To create assignments, proceed as follows:
   <ol type="a"><li>Choose **Add Assignment**.
   </li><li>In the **Role Collection** field, select **`ViewerRC`**.
   </li><li>Choose **Add Assignment** again.
   </li><li>Assign **`ManagerRC`** to your user in the same way.</li></ol>

### Test again - only GET operations should work


1. Restart the browser and open `https://<URL for the app router>/hw/users`.
2. To display the result, log in.
3. Try to perform a "change" operation, while considering the CSRF token, as explained in the previous tutorial.

    > A `403 Forbidden` message is returned because the test user has the **Viewer** role, that is, the **Display** scope, but not the **Update** scope contained in the **Manager** role.

### Enable change operations and retest


1. In the [SAP Cloud Platform cockpit](https://account.hanatrial.ondemand.com/#/home/welcome), choose **`<own Trial account>`** | **trial Subaccount** | **Security** | **Trust configuration** | **SAP ID Service**.
2. Enter your e-mail address in the **User** field, and choose **Show Assignments**.
3. Delete the assignment to **`ViewerRC`**.
4. Add an assignment to **`ManagerRC`**.
5. Restart the browser, and open `https://<URL for the app router>/hw/users`.
6. Log in and in Postman try to perform a "change" operation, while considering the CSRF token, as explained in the previous tutorial.

    > Now, you can do the change as all necessary authorizations are assigned to your user.

---
