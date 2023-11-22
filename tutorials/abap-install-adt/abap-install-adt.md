---
parser: v2
auto_validation: true
primary_tag: programming-tool>abap-development
tags: [  tutorial>beginner, programming-tool>abap-development, software-product>sap-business-technology-platform ]
time: 25
author_name: Merve Temel
author_profile: https://github.com/mervey45
---


# Download the Eclipse IDE and add the ABAP Development Tools (ADT) Plugin
<!-- description --> Download the Eclipse IDE and add the ABAP Development Tools (ADT) Plugin.


## You will learn
- How do download the Eclipse IDE
- How to add the ADT plugin to Eclipse


## Prerequisites
- Operating System: 
      - Windows 10, or higher
      - Apple macOS 10.15 or higher
- Microsoft VC++ Runtime:
      - For Windows: [Microsoft Visual C++ 2013 Redistributable (x64)](https://docs.microsoft.com/en-US/cpp/windows/latest-supported-vc-redist?view=msvc-170#visual-studio-2013-vc-120) 
    >**Hint:** Precisely version Visual Studio 2013 (VC++ 12.0) x64 is required.
-	Java Runtime:
      -	ADT is validated and tested against Java versions 17 and 21 (Oracle Java and `OpenJDK`).
      - The latest Eclipse packages are bundled with [`Eclipse Temurin`](https://adoptium.net/), an `OpenJDK` binary distribution provided by the [`Eclipse Adoptium`](https://projects.eclipse.org/projects/adoptium) project. Any other JRE found on the system is not used. If you want to remove the bundled JRE and use a custom one, see SAP note [3035242](https://launchpad.support.sap.com/#/notes/3035242).
    >**Hint:** You do not need to manually install a JRE / JDK, since it is already bundled with the latest Eclipse packages, which are downloadable as a ready-to-run zip file. 

---


### Download and run Eclipse IDE

  1. Open the [Eclipse download page](https://www.eclipse.org/downloads/packages/) to download the appropriate (usually latest) Eclipse version.

      ![eclipse](eclipse.png)

  2. Click **Download**.

      ![eclipse](eclipse2.png)

  3. Click the **Download** icon of your browser and select the folder symbol.

      ![eclipse](download0.png)

  4. Extract the Eclipse file with right-click.

      ![eclipse](download1.png)

  5. Click **Extract** in the wizard. 
   
      ![eclipse](download2.png)
  
  6. Open the **Eclipse-Java** folder.
     
      ![eclipse](download3.png)

  7. Open the **Eclipse** folder.

      ![eclipse](eclipse6.png)

  8. Double-click **`eclipse.exe`** to run the application.

      ![eclipse](eclipse7.png)

  9.  Launch your workspace.

      ![eclipse](eclipse8.png)

  10. Close both pages.

      ![eclipse](eclipse9.png)

      ![eclipse](eclipse10.png)


### Add ADT plugin to Eclipse

 1. Select **Help** > **Install New Software**.

      ![eclipse](eclipse11.png)

 2. Enter the latest ADT URL **`https://tools.hana.ondemand.com/latest`** in the **Work with** section, press enter,  select **ABAP Development Tools** and click **Next >**.

      ![eclipse](adt.png)

 3. Click **Next >**.

      ![eclipse](adt2.png)

 4. Accept the **license agreement** and click **Finish**.

      ![eclipse](adt3.png)


 5. Click **Select All** and **Trust Selected**.

      ![eclipse](adt4.png)

 6. Click **Restart Now**.

      ![eclipse](eclipse17.png)

 7. Now ADT is installed. Switch to the ABAP perspective. Therefore select **Window** > **Perspective** > **Other Perspective** > **Other**.

    ![eclipse](perspective.png)

    Then select **ABAP** and click **Open**.

      ![eclipse](perspective2.png)

 8. Check your result.

      ![eclipse](eclipse18.png)


### More information

Following this tutorial you will be able to update the latest version of Eclipse and ADT when new releases are available.

If you want to install the abapGit plugin for ADT, you can follow the [Install the abapGit Plugin](abap-install-abapgit-plugin) tutorial.

### Test yourself



