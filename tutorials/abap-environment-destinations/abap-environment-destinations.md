---
parser: v2
auto_validation: true
time: 15
tags: [ tutorial>beginner, programming-tool>abap-development, products>sap-business-technology-platform]
primary_tag: products>sap-btp--abap-environment
---

# Create destinations in SAP BTP cockpit
<!-- description --> Create Design- and Runtime destinations for Content Provider creation and dynamic oData and iFrame access.

## Prerequisites
 - **Authorizations:** Your user needs to be a subaccount administrator user in SAP BTP cockpit for the subaccount the destinations will be created in.

## You will learn
  - How to create destinations
## We will create
  - One Design-time destination in order to define the location from which the SAP Launchpad service should fetch the exposed content.

  - Two runtime destinations.

      - One for fetching data for dynamic tiles.

      - One for launching apps in an `iFrame`.

---

### Create Designtime destination

- Go to the subaccount, in which you want to use the  Launchpad Service.

- Choose **Destinations** in the **Connectivity** menu item.

- Create a new Destination.

<!-- border -->![Settings for new Destination](1-Destination-settings.png)

**URL**:

- Use Service URL of section "Inbound Services" in the Communication Arrangement created in tutorial ["Create communication between SAP BTP, ABAP environment and SAP BTP"](abap-environment-communication).

- Add suffix `/entities`.

- Format: `https://<guid>.abap.<region>.hana.ondemand.com/sap/bc/http/sap/aps_flp_content_exposure/entities`

**User**: Inbound user with its password created in tutorial ["Create communication between SAP BTP, ABAP environment and SAP BTP"](abap-environment-communication).

Press "Save".


### Create Runtime Destination for launching apps in an iFrame

Create a new Destination.

<!-- border -->![Settings for Runtime iFrame integration](3-Destination-iFrame.png)

**URL**: Use API-URL of section "Common Data" in the Communication Arrangement created in tutorial ["Create communication between SAP BTP, ABAP environment and SAP BTP"](abap-environment-communication). Replace `abap` with `abap-web`. (Format: https://<`tenant`>.**abap-web**.<`region`>.hana.ondemand.com)

**Additional Properties**:

**`HTML5.DynamicDestination`**: `true`

**`sap-platform`**: `ABAP`

Press "Save".


### Create Runtime Destination for dynamic oData access
 <br>
Create a new Destination.

<!-- border -->![Settings for dynamic Runtime Destination](2-Destination-oData.png)

**URL**: API-URL of section "Common Data" in the Communication Arrangement created in tutorial ["Create communication between SAP BTP, ABAP environment and SAP BTP"](abap-environment-communication).<br>
Format: https://<`tenant`>.**`abap`**.<`region`>.hana.ondemand.com

**URL**: API-URL of section "Common Data" in the Communication Arrangement created in tutorial ["Create communication between SAP BTP, ABAP environment and SAP BTP"](abap-environment-communication). Replace `abap` with `abap-web`.
(Format: https://<`tenant`>.**abap-web**.<`region`>.hana.ondemand.com)

**`AuthnContextClassRef`**: `urn:oasis:names:tc:SAML:2.0:ac:classes:PreviousSession`

**Additional Properties**:

**`HTML5.DynamicDestination`**: `true`

**`nameIdFormatCreate`**: `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`

Press "Save".


---
