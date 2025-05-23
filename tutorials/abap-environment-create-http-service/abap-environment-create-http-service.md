---

parser: v2
auto_validation: true
time: 15
tags: [ tutorial>beginner, software-product>sap-btp--abap-environment, software-product>sap-business-technology-platform, software-product-function>s-4hana-cloud-abap-environment, tutorial>license]
primary_tag: programming-tool>abap-development
author_name: Julie Plummer
author_profile: https://github.com/julieplummer20
---

# Create an HTTP Service in SAP BTP ABAP Environment
<!-- description --> Create an HTTP service in SAP BTP ABAP environment that can be called from the browser.

## Prerequisites

- **IMPORTANT**: This tutorial cannot be completed on a trial account
- You have set up SAP Business Technology Platform (BTP), ABAP Environment, for example by using the relevant booster: [Using a Booster to Automate the Setup of the ABAP Environment](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/cd7e7e6108c24b5384b7d218c74e80b9.html)
- **Tutorial**: [Create Your First Console Application](abap-environment-trial-onboarding), for a licensed user, steps 1-2
- You have installed [ABAP Development Tools](https://tools.hana.ondemand.com/#abap), latest version


## You will learn  
  - How to create an HTTP service that can be accessed from a browser
  -	How to return system data using a (whitelisted) ABAP utility class
  - How to expose the service for external consumption, by defining the necessary inbound communication artifacts

Important: This tutorial is not suitable for SAP S/4HANA Cloud, private edition. If you would like to enable HTTP consumption from S/4HANA Cloud, private edition, see:

- [Developing External Service Consumption - Outbound Communication](https://help.sap.com/docs/ABAP_PLATFORM_NEW/b5670aaaa2364a29935f40b16499972d/f871712b816943b0ab5e04b60799e518.html)
- [Enable HTTP Communication in Your ABAP Code](https://help.sap.com/docs/ABAP_PLATFORM_NEW/b5670aaaa2364a29935f40b16499972d/cef1ada754154d11b5701ab60e6ab412.html?version=202310.002)


Throughout this tutorial, replace `XXX` or `000` with your initials or group number.

---


### Create an HTTP service

1. Select a package and choose **New > Other Repository Object** from the context menu:

    <!-- border -->
    ![Image depicting step-1a-new-repo-object](step-1a-new-repo-object.png)

2. Enter the filter text **HTTP** and choose **Next**:

    <!-- border -->
    ![Image depicting step-1b-choose-HTTP-service](step-1b-choose-HTTP-service.png)

3. Enter a **Name:`Z_GET_DATE_HTTP_000`** and **Description:Get system date** for your service and choose **Next**:

    <!-- border -->
    ![Image depicting step-1c-name-service](step-1c-name-service.png)

4. Choose or create a **transport request**:

    <!-- border -->
    ![Image depicting step-1d-transport-request](step-1d-transport-request.png)

The new HTTP service is displayed on a new tab. The handler class and URL are generated automatically, in the form:
**`https://<server:port>/sap/bc/http/sap/<service_name>?sap-client=100`**

<!-- border -->
![Image depicting step-1e-new-service-created](step-1e-new-service-created.png)


### Implement the handler class

Now, you will implement the handler class, starting with a simple text output.

1. Open the handler class by clicking on the hyperlink:

    <!-- border -->
    ![Image depicting step-2a-open-handler-class](step-2a-open-handler-class.png)

2. The structure of the class and the interfaces statement for `IF_HTTP_SERVICE_EXTENSION` are generated automatically.

3. Go to the class implementation section and insert the following statement in the method:

    **`response->set_text('Hello!').`**

    <!-- border -->
    ![Image depicting step-2b-insert-method](step-2b-insert-method.png)

4. **Save (`Ctrl+S`)** and **Activate (`Ctrl+F3`)** your class.


### Test the service

1. Go back to your HTTP Service. Test your service in the browser by clicking the URL link:

    <!-- border -->
    ![Image depicting step-4-test-http-service](step-4-test-http-service.png)

2. If necessary, log in again. The preview open automatically in a new tab and display something like this:

    <!-- border -->
    ![Image depicting step-4b-hello](step-4b-hello.png)


### Add method `get_html`

Now you will add a method to get system data and format this in HTML

In the ABAP environment, you can only use whitelisted APIs. Therefore, for example, you cannot use `SY-UNAME`. Instead, you call the appropriate method of the class `CL_ABAP_CONTEXT_INFO`.

1. In your class definition, add the following statement:

    ```ABAP
    METHODS: get_html RETURNING VALUE(ui_html) TYPE string.

    ```
2. You will get the error "Implementation missing...". Resolve this by choosing **Quick Assist ( `Ctrl+1` )** and choosing **Add implementation...**. Ignore the two warnings for now.

    ```ABAP

    DATA(system_date) = cl_abap_context_info=>get_system_date( ).

    ui_html =  |<html> \n| &&
    |<body> \n| &&
    |<title>General Information</title> \n| &&
    |<p style="color:DodgerBlue;"> Hello there </p> \n | &&
    |<p> Today, the date is:  { system_date }| &&
    |<p> | &&
    |</body> \n| &&
    |</html> | .

    ```

3. Now change the method implementation of the method **`handle_request`**

    ```ABAP
    response->set_text( get_html(  ) ).

    ```

4. Now select the warning for the method **`get_html`** and choose **Quick Assist `( Ctrl + 1 )`**. (You cannot resolve the warning for the method `handle_request`).

5. Choose **Add raising declaration**, then choose **Finish**.

6. Format, save, and activate the class **( `Shift + F1, Ctrl + S, Ctrl + F3` )**.


### Test the service again (optional)

Your output should look roughly like this:

<!-- border -->
![step8a-formatted-html](step8a-formatted-html.png)


### Check code

Your code should look like this:

```ABAP
CLASS Z_GET_DATE_HTTP_000 DEFINITION
  PUBLIC
  CREATE PUBLIC .

  PUBLIC SECTION.

    INTERFACES if_http_service_extension .

    METHODS: get_html RETURNING VALUE(ui_html) TYPE string
    RAISING
        cx_abap_context_info_error.

  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS Z_GET_DATE_HTTP_000 IMPLEMENTATION.



  METHOD get_html.

    DATA(system_date) = cl_abap_context_info=>get_system_date( ).

    ui_html =  |<html> \n| &&
    |<body> \n| &&
    |<title>General Information</title> \n| &&
    |<p style="color:DodgerBlue;"> Hello there.</p> \n | &&
    |<p> Today, the date is:  { system_date }| &&
    |<p> | &&
    |</body> \n| &&
    |</html> | .

  ENDMETHOD.

  METHOD if_http_service_extension~handle_request.
    TRY.
        response->set_text( get_html(  ) ).
      CATCH cx_web_message_error cx_abap_context_info_error.
        "additional exception handling
    ENDTRY.
  ENDMETHOD.

ENDCLASS.
```

### Create an inbound Communication Scenario

You will now create the artifacts you need to allow other systems to call your service compliantly. This involves some overhead for one consumer; however, the advantage is that you can add several consumer systems, or users (for example, with different authentication) pointing to the same HTTP service, wrapped in the same Communication Scenario.

<!-- border -->
![step9-create-comm-artefacts-overview](step9-create-comm-artefacts-overview.png)

 First, create the **Communication Scenario**.

1. Select your package, then choose **New > Other Repository Object...** from the context menu.

    <!-- border -->
    ![step9a-new-other](step9a-new-other.png)

2. Add the filter **`scen`**, then choose **Communication Scenario**, then choose **Next**.

    <!-- border -->
    ![step9c-create-comm-scenario](step9c-create-comm-scenario.png)

3. Add a **Name: `Z_WRAP_HTTP_INBOUND_000`** and **Description**, choose a transport request, then choose **Finish**.

Your Communication Scenario appears.

<!-- border -->
![step9-new-comm-scen](step9-new-comm-scen.png)


### Add the HTTP service

1. On the **Inbound** tab, choose **Add...**.

    <!-- border -->
    ![step10-add-http-service](step10-add-http-service.png)

2. **IMPORTANT**: Choose **Browse**. You cannot simply enter the name. Then add a filter, such as **`Z_HTTP`**, select your service, then choose **Finish**.

    <!-- border -->
    ![step10a-browse-for-service](step10a-browse-for-service.png)

3. Your service appears. Choose **Publish Locally**.

    <!-- border -->
    ![step10-b-publish-locally](step10-b-publish-locally.png)


### Create Communication Arrangement

1. Open the SAP Fiori launchpad for your system. You can find the URL for this launchpad by selecting your system (that is, ABAP Project) in Project Explorer, then choosing **Properties > ABAP Development** from the context menu.

    <!-- border -->
    ![step11a-open-flp](step11a-open-flp.png)

2. From the launchpad, choose the Fiori app **Communication Arrangement**. Then choose **New**.

    <!-- border -->
    ![step11b-new](step11b-new.png)

3. Choose your scenario, **`Z_WRAP_HTTP_INBOUND_000`** from the drop-down list. Accept the default (identical) Arrangement name.

    <!-- border -->
    ![step11c-name-comm-arr](step11c-name-comm-arr.png)



### Create Communication System

1. From the Dashboard Home screen, choose **Communication Systems**.

2. Enter the name of your **Communication Arrangement**, then for **Communication System**, choose **New**.

3. Enter a **System ID** and Accept the default (identical) System name, then choose **Create**.

4. In **Technical Data > General > Host Name**, enter **Dummy**. Leave the other defaults and choose **Save**.

    <!-- border -->
    ![step12a-new-system](step12a-new-system.png)


### Create Communication User

1. Scroll down to **Users for Inbound Communication**, then create a new user by choosing the **+** icon.

    <!-- border -->
    ![step13a-create-comm-user](step13a-create-comm-user.png)

2. Choose **New User** and the **Authentication Method: User name and password**.

    <!-- border -->
    ![step13b-choose-new-user](step13b-choose-new-user.png)

3. Enter a name and description, then choose **Propose password**, then choose **Create > OK > Save**.


### Save Communication Arrangement


<!-- border -->
![step14-save-comm-arr](step14-save-comm-arr.png)



### Test yourself




### More Information

- [SAP Help Portal: HTTP Communication](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/3884bc38209843ac900d92adb9c2a863.html)

- [SAP Help Portal: Components of SAP Communication Technology - HTTP Service](https://help.sap.com/doc/saphelp_nwpi71/7.1/en-US/1f/93163f9959a808e10000000a114084/frameset.htm)

- [SAP ABAP Keyword Documentation: Calling an HTTP Service](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/index.htm): Search for **ICF, Subject**

---
