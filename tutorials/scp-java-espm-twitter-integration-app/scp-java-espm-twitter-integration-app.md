---
parser: v2
primary_tag: programming-tool>java
tags: [  tutorial>intermediate, topic>cloud, programming-tool>java, products>sap-cloud-platform ]
---

# Running the ESPM Twitter Integration sample app on SAP Cloud Platform
<!-- description --> Learn how to download, build, deploy, configure and run the ESPM Twitter Integration JAVA sample app on SAP Cloud Platform

## Prerequisites  
 - **Proficiency:** Intermediate
 - Basic knowledge about JAVA and OAuth authentication.
 - [Oracle Java SE Development Kit (JDK) 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
 - [Eclipse Neon](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/761374e5711e1014839a8273b0e91070.html)
 - [SAP Development Tools for Eclipse](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/76137a37711e1014839a8273b0e91070.html)
 - [SAP Cloud Platform Neo Environment SDK, Java Web Tomcat 8](https://tools.hana.ondemand.com/#cloud)
 - [Set Up the Runtime Environment in Eclipse, Java Web Tomcat 8 Runtime](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/7613f000711e1014839a8273b0e91070.html)
 - [Sign up for a free trial account on SAP Cloud Platform](https://developers.sap.com/tutorials/hcp-create-trial-account.html)


## You will learn  
This cool sample app will show you how to integrate Twitter to your Java app. It uses Twitter for OAuth authorization and show how to call a Twitter API to publish a tweet from your app.
## Time to Complete
**20 Min**

---

## Intro
In this app, customers log in using Twitter as OAuth authorizer. Using Twitter ID, they can shop and tweet about the products they bought.

These are the steps to download, build, deploy, configure and run our Twitter Integration app.

### Create ESPM Application in Twitter

1. Register ESPM Web Shop application in the developer or API portion of Twitter website (https://apps.twitter.com) by clicking on **Create New App**.
2. Enter the **Application Name** (for example: `Twitter_Integration`), **Description**, **Website** (for example: `https://www.sap.com`), **Callback URL** (for example: `https://www.sap.com`), then click on **Create your Twitter Application** button.
>**Note**: We need to set a sample callback URL to retrieve the Request Token and get the Access Token with `OAuth_verifier` parameter, which will be added to the actual callback URL in the application upon successful login.
3. Click on **Keys and Access Tokens** tab, then take note of **Consumer Key** and **Consumer Secret** key.

### Configuring Maven in Eclipse

1. in Eclipse, click on menu **Window** > **Preferences** if you are on Windows, or press Command + , if you are on Mac.
2. Navigate to **Maven** > **User Settings**.
3. Click on **Open file** under **User Settings**. If you do not have one, create the file `/Users/your-user-name/.m2/settings.xml`.
![Configure maven eclipse image](configure-maven-eclipse.png)
4. The contents of the `settings.xml` file should look like the snippet below.  

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
	<localRepository>${user.home}/.m2/repository</localRepository>
	<profiles>
		<profile>
			<id>development</id>
			<activation>
				<activeByDefault>true</activeByDefault>
			</activation>
			<properties>
			</properties>
		</profile>
	</profiles>
	<proxies>
		<proxy>
			<active>true</active>
			<protocol>http</protocol>
			<host>proxy</host>
			<port>8080</port>
		</proxy>
	</proxies>
</settings>  
```

>**Note**: For most people, the proxy value does not need to be set, so you can remove the entire proxy section from the snippet, but if you are working behind a proxy, then it should be set as per your environment.

### Configuring Git in Eclipse

1. in Eclipse, click on menu **Window** > **Preferences** if you are on Windows, or press Command + , if you are on Mac.
2. Navigate to **Team** > **Git** > **Configuration**.
3. Add the following configuration:
![Git configuration image](git-configuration.png)
>**Note**: For most people the proxy value does not need to be set, but if you are working behind a proxy, then it should be set as per your environment.

### Cloning Git repository and importing Maven project

1. Open <https://github.com/SAP/cloud-espm-v2/tree/Twitter_Integration> with your web browser.
2. Click on **Clone or download**, then on Copy to clipboard icon.
![Github copy clipboard image](github-copy-clipboard.png)
3. In Eclipse, go to menu **Window** > **Perspective** > **Open Perspective** > **Other...**.
![Open perspective image](open-perspective.png)
4. Select **Git** and press **OK**.
![Open git perspective image](open-git-perspective.png)
5. In **Git Repositories** box, you will see three links. Click on the **Clone a Git repository** link.
![Git perspective image](git-prespective.png)
6. Because you copied before the project repository URL to your clipboard, the fields of the opened dialog are filled automatically with the correct values. So do not change anything, just click on **Next >**.
![Clone repository 1 image](clone-repository1.png)
7. Select only the `Twitter_Integration` branch, then press **Next >**.
![Branch selection image](branch-selection.png)
8. On the last wizard page you can adjust the location of the local Git Repository, but for the scope of this tutorial we will just leave the default as-is. Copy the path and click on **Finish**.
![Clone repository 2 image](clone-repository2.png)
9. Go to menu **File** > **Import** > **Maven** > **Existing Maven Projects**.
![Import maven project 1](import-maven-project1.png)
10. Paste the path of your downloaded project.
![Import maven project 2](import-maven-project2.png)
11. Click on **Finish**.

### Building the Maven project

1. In Eclipse, go to menu **Window** > **Perspective** > **Open Perspective** > **Other...**.
![Open perspective image](open-perspective.png)
2. Select **Java EE** and press **OK**.
![Select JavaEE perspective image](select-javaee-perspective.png)
3. In the **Project Explorer** window, right click on `espm` project folder and choose **Maven** > **Update Project**.
![Update maven project image](update-maven-project.png)
4. Press **OK** and wait until the update is finished.
![Update maven project 2 image](update-maven-project2.png)
5. In the **Project Explorer** window, right click on `espm` project folder and choose **Run As** > **Maven build...**.
![Maven build image](maven-build.png)
6. In the goals field, write `clean install`, then click on **Run**.
![Maven goals image](maven-goals.png)
7. In the **console** window, you should have a **BUILD SUCCESS** info.
![Build success image](build-success.png)
8. Update your project again like step 3.

### Deploying the application on SAP SCP via the cockpit

1. Log in to SCP on your trial account, then click on **Applications** > **Java Application** > **Deploy Application**.
![Deploy app image](deploy-application.png)
2. Set the fields as described below, then click on Deploy.

  Field Name         | Value
  ------------------ | ------------------
  War File Location  | `<Your_Project_Folder>/espm-cloud-web/target/espm-cloud-web.war`
  Application Name   | `espm`
  Runtime Name       | `Java Web Tomcat 8`
  JVM Version        | `JRE 8`
  ![Pick war file image](pick-war-file.png)
3. Wait until the deploy process is finished, but do not start the app yet.

### Setting up Destination for Twitter Integration

1. In SCP Cockpit, click on **Applications** > **Java Applications** > your app (`espm`) > **Configuration** > **Destination**.
![SCP java apps image](scp-java-apps.png)
2. Click on **New Destination** and then fill out the form with the following info:

  Field Name         | Value
  ------------------ | ------------------
  Name                                       | `twitterOauth`
  Type                                       | `HTTP`
  URL                                        | `<Any_Website_URL>`
  Proxy Type                                 | `NoAuthentication`
  `consumerApplicationKey` (new property)    | `<Consumer_Key>` (Twitter, step 1)
  `consumerApplicationSecret` (new property) | `<Consumer_Secret>` (Twitter, step 1)
  ![App destination image](app-destination.png)
3. Click on **Save**.

### Binding the database to espm application

1. On left menu of SCP cockpit, click on **SAP HANA / SAP ASE** > **Databases & Schemas**.
2. Click on the new button
![SCP database schemas image](scp-database-schemas.png)
3. In the pop-up window, enter the **Schema ID** as `espm` and leave the **Database System** as `HANA <shared>`.
4. Click on **Save**.
![SCP new database image](scp-new-database.png)
5. Go back to the main cockpit page, then click on **Applications** > **Java Applications** > your app (`espm`) > **Configuration** > **Data Source Bindings**.
![SCP java apps image](scp-java-apps.png)
6. Click on **New Binding**.
7. In the pop-up window, set the **DB/Schema ID** as `espm`, then click on **Save**.
![Datasource binding image](datasource-binding.png)

### Testing your application

1. In the context menu on the left, click on **Overview**.
2. Click on **Start** and wait until the status changes to **Started**.
3. Click on the **Application URL**.
![Java app overview image](java-app-overview.png)
4. You will be authenticated with your Twitter credentials. If you are already logged in, you will see a Twitter screen informing that you will be redirected, otherwise enter your Twitter credentials.
5. Add a product to your chart.
![App initial screen image](app-initial-screen.png)
6. Click on the chart icon.
![App chart image](app-chart.png)
7. Click on **Go to checkout**. Press **Step 2** and fill out all the fields, then click on **Step 3**.
![App name and address image](app-name-address.png)
8. Click on **Review**, then **Place an order**.
![App summary image](app-summary.png)
9. Click on Tweet and check in your Twitter page your app's tweet.
