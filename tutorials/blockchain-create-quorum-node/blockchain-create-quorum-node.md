---
parser: v2
time: 15
tags: [tutorial>beginner, products>sap-cloud-platform, topic>cloud, topic>blockchain]
primary_tag: topic>Blockchain
author_name: José Prados
author_profile: https://github.com/joseprados

---

# Create a Quorum Development Node
<!-- description --> Understand the Quorum service and learn how to create and manage nodes and service keys on the SAP Cloud Platform.

## You will learn
  - About the available Quorum service plans on SAP Cloud Platform
  - How to provision a Quorum development node
  - How to create and maintain Quorum service keys from your Quorum dashboard

---

### Understand the Quorum service


 The Quorum service is integrated into SAP Cloud Platform via a service broker. The service broker supports several node types as service plans.

 **Development Nodes**

 The simplest node type is a development node. This is effectively an environment with all relevant APIs for developing and testing of Solidity smart contracts on a fixed network with shared nodes. This node type has private transactions disabled because of the nature of shared nodes.

![Image depicting Development Node on SAP Cloud Platform](06--DevNode.png)

**`Testnet` Nodes**

For more complex testing of real-world scenarios, `Testnet` nodes are supported. In this network, the Quorum nodes are part of a cross organizational test network available for developing and testing distributed applications, including private transactions. Partners and customers can provision single nodes into this network creating a testing a specific private test scenario using private transactions.

![Image depicting Testnet Node on SAP Cloud Platform](07--TestnetNode.png)


**Connect Your Own Network**

The Connect Your Own Network (`CYON`) plan supports consortium cases where the complete Quorum network is provisioned and operated outside of the SAP Cloud platform (for example centrally by the consortium), and where one peer node needs to be made accessible from SAP Cloud Platform. This service plan enables the transparent connection of SAP business processes onto this external network.

![Image depicting Connect Your Own Network on SAP Cloud Platform](09--CYON.png)

For our `Hello World` example we will be using a Development Node, with the steps provided for provisioning this below:


### Open the Quorum service


Once on the SAP Cloud Platform Service Marketplace, locate and open the Quorum service by clicking the relevant service tile.

![Image depicting SAP Cloud Platform marketplace](01--SCP-ServiceMarketplace.png)


### Navigate to Instances


Once in the Quorum service, you will see a service description and the available plans.

For the `Hello World` example, we will be using the `dev` plan. This is effectively a single standalone service instance (often referred to as a node) that can be used to develop and test Solidity smart contracts. To keep costs to a minimum, developer nodes are hosted in a multi-tenant fashion on nodes operated by SAP.

Click the **Instances** tab on the side menu, opening an overview of available Quorum instances in your subaccount:

![Image depicting Quorum service overview](02--Quorum-Service-Overview.png)


### Create new instance


Once on your Quorum instances overview, click **New Instance** to open the service instance wizard:

![Image depicting Quorum service instances overview](03--Quorum-Instance-Overview.png)


### Choose service instance settings


Navigate through the service instance wizard, selecting the following settings:

Field | Value
:------|:--------
**Plan**  | `Dev`
Specify Parameters | N/A
Assign Application | N/A
**Name** | `Hello World Demo Story`

After selecting the settings, click **Finish**.

![Image depicting Quorum service instance wizard](04--Quorum-Create-Instance.png)


### Confirm creation of node


Your development node will now be provisioned (which may take a few moments) and displayed on the overview of available service instances.

![Image depicting Quorum node provisioned](05--Quorum-Node-Created.png)


### Access your Quorum node dashboard


After creating a Quorum service instance, provisioning a node in the process, you have access to your individual node dashboard. This displays the status of your node information.

![Image depicting Quorum node dashboard icon](06--Quorum-Node-Accessing-Dashboard.png)


### Understand your Quorum node dashboard


Your Quorum dashboard provides read-only access to node information, network configuration information, and network logs. You can also use your Quorum dashboard to create and manage node service keys.

![Image depicting Quorum node dashboard](07--Quorum-Node-Understanding-Dashboard.png)

When accessing your Quorum dashboard you can view the following information:

**Node Information**

![Image depicting Quorum node information in dashboard](08--Quorum-Node-Dashboard-Node-Information.png)

**Network Configuration**

On the Network Configuration page you can view your basic settings, available peers in the network, and your genesis .JSON file:

![Image depicting Quorum network configuration information in dashboard](08--Quorum-Node-Dashboard-Network-Configuration.png)

**Logs**

This gives you read-only information to your network logs, including timestamp, level, and text information. You can manually refresh the table by clicking the **Refresh** button.


### Create and manage Quorum service keys


Service keys for your Quorum node can be created and managed directly on your Quorum dashboard. Upon creation of a service key, a new blockchain account is created on the SAP Cloud Platform and a password is provided at the time of creation.
From your Quorum dashboard, you can create a service key by scrolling to the service key section and clicking **Add**.

![Image depicting Quorum node dashboard](10--Quorum-Node-Dashboard-Node-Create-Service-Key.png)

Now enter an internal service key name, provide and confirm an account password, and then click **Create**

![Image depicting Quorum node dashboard](11--Quorum-Node-Dashboard-Node-Create-Service-Key.png)

Your service key is now created and can be viewed and managed directly on your dashboard.

In this service key you can see:

**address** - This is the `geth` blockchain account address.

**RPC** - A unique RPC endpoint.

![Image depicting Quorum node dashboard](11--Quorum-Node-Dashboard-Node-Create-Service-Key-Info.png)

