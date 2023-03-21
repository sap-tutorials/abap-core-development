---
parser: v2
primary_tag: topic>internet-of-things
tags: [ tutorial>beginner, tutorial>license, topic>internet-of-things, topic>cloud, products>sap-leonardo-iot, product>sap-edge-services, products>sap-cloud-platform-internet-of-things, products>sap-cloud-platform ]
---

# Send Data with REST Edge
<!-- description --> Send data to the SAP Cloud Platform Internet of Things Service using the Internet of Things Gateway Edge REST.

<!-- loioee7177e2b96a41138f9779df6dfa0867 -->

## Prerequisites
 - **Proficiency:** Beginner
 - **Tutorials:** You have completed [Install the Paho Client] and [Create a Device Model Using the API] (You need to create a REST device, a capability and a sensor type).

## You will learn
- How to set up the Internet of Things Gateway Edge with the HTTP REST adapter
- How to and send data to the SAP Cloud Platform Internet of Things Service using the Internet of Things Gateway Edge with the HTTP REST adapter
- How to use cURL as a sample client for REST
## Time to Complete
20 min

---

### Download the Internet of Things Gateway Edge (and Adapters)


You can download the Internet of Things Gateway Edge from the SAP Software Center. The downloaded software includes the adapters for all protocols supported.

1.  Log on to the [SAP Software Center](https://launchpad.support.sap.com/#/softwarecenter).

2.  Search for [IoT Services Edge](https://launchpad.support.sap.com/#/softwarecenter/search/iot%2520services%2520edge).

3.  Download the `IOTCHCP**_*-70002561.ZIP` and extract it on your machine.

4.  Inside the extracted folder, you can find another ZIP archive. Please extract this `gateway-<version>.zip`.

### Set Up the Internet of Things Gateway Edge


1.  Open the terminal (macOS) or command line tool CMD (Windows) and change the directory to the path of the extracted ZIP file Internet of Things Gateway Edge.

    The directory must contain the following files: `build.bat` or `build.sh` (executable).

2.  Launch the `build.bat` (Windows) or the `build.sh` (macOS) in the extracted folder.

> Note:
> To launch the `build.bat` on Windows an installation of Java Development Kit (JDK) is required.
>
>

    For Windows:

    ```bash
    build.bat REST
    ```

    For Mac:

    ```bash
    ./build.sh REST
    ```

    A new file `config_gateway_rest.xml` is created in the `/config` folder.

3.  **Optional:** Copy custom OSGi bundles from a former installation of the Internet of Things Gateway Edge by adding its file path as a build parameter.

    Launch `build.bat` (Windows) or `build.sh` (macOS) in the extracted folder.

    > Note:
    > All previously installed custom OSGi bundles of the Internet of Things Gateway Edge are automatically installed to the new version.
    >
    >

    For Windows:

    ```bash
    build.bat REST "path\to\your\previous\installation"
    ```

    For Mac:

    ```bash
    ./build.sh REST "path/to/your/previous/installation"
    ```

4.  Open the `config_gateway_rest.xml` with a text editor.

5.  Enter the **<HOST_NAME>** for the `connectionString` by replacing `127.0.0.1` and for the address element in the `coreConnection` by replacing `localhost`.

    ```bash
    <cnf:connectionString>failover:(nio+ssl://sample.cp.iot.sap:61616?daemon=true&amp;soTimeout=60000)</cnf:connectionString>

    ```

    ```bash
    <cnf:coreConnection>
    <cnf:address>https://sample.cp.iot.sap:443/iot/core/api/v1/</cnf:address>
    <cnf:timeout>60000</cnf:timeout>
    <cnf:authentication>BASIC_AUTH</cnf:authentication>
    <cnf:mutual>false</cnf:mutual>
    </cnf:coreConnection>
    ```

6.  **Optional:** Add the attribute `gatewayAlternateId` to the `<cnf:gateway>` tag.

    ```bash
    <cnf:gateway gatewayAlternateId="1122334455667788">
    ```

    > Note:
    > In case of problems on gateway startup due to gateway alternate Ids being used multiple times in the Internet of Things Service Cockpit, the `gatewayAlternateId` of the gateway to be created can be changed by adding this attribute. Set the attributes value to a valid alphanumeric string.
    >
    >

7.  Save your changes to the file.

8.  Open the `rest.json` file located in folder `/config/restConfig` with a text editor.

9.  Set `serverIp` to the IP of the system where the Internet of Things Gateway Edge is running.

    ```bash
    "serverIp" : "192.168.1.1"
    ```

    > Note:
    > In case the Internet of Things Gateway Edge is running on `localhost,` leave it at `127.0.0.1` . The default port for the REST API is `8699` (if necessary change it by setting the value of the field `serverPort`).
    >
    >

    > Note:
    > The Internet of Things Gateway Edge binds only to the address given in `serverIp`. Specify one of your local IP addresses here. The address you bind the Internet of Things Gateway Edge to and the address through which you access the Internet of Things Gateway Edge can be different, depending on your network topology, for example, reverse proxies.
    >
    >

10. Set `useDeviceAuth` to **`false`**.

11. Save your changes to the file.

### Download the Certificate


1.  Log on to the Internet of Things Service Cockpit with your user credentials.

    ```bash
    https://<HOST_NAME>/iot/cockpit/
    ```

2.  Choose the show/hide user information button in the upper right corner.

    The system displays the*Name*, *Tenant*, *Tenant Role* and a *Certificate* Download of your user.

3.  Choose *Download*. The system starts to download a ZIP file, named `certificate-<user>.zip` containing the certificates.

4.  Create a `certificates` folder inside the `config` folder manually first. Extract the `certificate-<user>.zip` file inside the newly created `certificates` folder.

5.  Open the `pswd.properties` file in the certificates folder with a text editor.

6.  Enter your user password as password.

    ```bash
    password = <`password`>
    ```

7.  Save your changes to the file.

### Start the Internet of Things Gateway Edge


1.  Open the terminal (macOS) or command line tool CMD (Windows) and change the directory to the path of the extracted ZIP file Internet of Things Gateway Edge.

    The directory must contain the following files: `build.bat` or `build.sh` (executable).

2.  Launch the `gateway.bat` (Windows) or the `gateway.sh` (macOS) located in the root of the extracted folder.

    For Windows:

    ```bash
    gateway.bat
    ```

    For Mac:

    ```bash
    ./gateway.sh
    ```

    If the Internet of Things Gateway Edge is started successfully, you see the following message *Nodes successfully retrieved, start node configuration*.

    > Note:
    > You must be connected to public Internet. Most corporate networks do not work due to port and protocol restrictions.
    >
    >

### Publish Data With cURL Using REST


**Prerequisites:**

-   You have installed the REST client (cURL). A description of how to install the Paho client can be found in the tutorial [Install the Paho Client].

-   You have created a REST device, a capability, and a sensor type as described in the tutorial [Create a Device Model Using the API]. You wrote down the Alternate ID of the created device, capability, and sensor type.


1.  Open the terminal (macOS) or command line tool CMD (Windows).

    > Note:
    > If you did not add the cURL directory to your PATH environment variable you need to switch to the cURL directory.
    >
    >

2.  Enter and send the message string.

    ```bash
    curl -v -H "Content-Type: application/json" -d "{ \"capabilityAlternateId\": \"<capabilityAlternateId>\", \"sensorTypeAlternateId\": \"<sensorTypeAlternateId>\", \"measures\": [[\"<MEASURE>\"]] }" <DEVICE_ALTERNATE_ID>
    ```

    -   Add the parameter `capabilityAlternateId` with the alternate id of the previously created capability.

    -   Add the parameter `sensorTypeAlternateId` with the alternate id of the created sensor type. (A sensor associated to the new sensor type will be automatically created.)

    -   Add the parameter `measures` with the array of measures.

    Two `measures` formats are allowed.

    -   One with the measures specified as array of array:

        ```bash
        curl -v -H "Content-Type: application/json" -d "{ \"capabilityAlternateId\": \"ba19f8b6584cf3bd\", \"sensorTypeAlternateId\": \"1\", \"measures\": [[\"25\"]] }"
        http://<IP_GW_EDGE>:<PORT_GW_EDGE>/measures/<DEVICE_ALTERNATE_ID>
        ```

    -   Another one, specifying measures as array of json objects where the name of each property defined in the capability is the key:

        ```bash
        curl -v -H "Content-Type: application/json" -d "{ \"capabilityAlternateId\": \"ba19f8b6584cf3bd\", \"sensorTypeAlternateId\": \"1\", \"measures\": [{\"temperature\": \"25\"}] }"
        http://<IP_GW_EDGE>:<PORT_GW_EDGE>/measures/<DEVICE_ALTERNATE_ID>
        ```

3.  Set `<IP_GW_EDGE>` to the IP by which the server, where the Internet of Things Gateway Edge is running, can be accessed from outside the network.

    > Note:
    > In case the Internet of Things Gateway Edge is running on `localhost`, set it to `127.0.0.1` .
    >
    >

4.  Set `<PORT_GW_EDGE>` to the same value specified in the `config\restConfig\rest.json` file (default value is `8699`).

5.  Enter the recorded `<DEVICE_ALTERNATE_ID>` as a string. For example: `d6e50978854c0f9b`.

6.  Send the message.

7.  A message is sent to the Internet of Things Service.

8.  You can check the incoming values using the *Data Visualization* of the device in the Internet of Things Service Cockpit or the Internet of Things API Service. For more information, please refer to the tutorial [Consuming Measures.](https://help.sap.com/viewer/7e269da75d024ef09bfb7a5986c47517/Cloud/en-US)
