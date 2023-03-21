---
parser: v2
author_name: Katja Geiselhart
author_profile: https://github.com/D054382
auto_validation: true
time: 20
tags: [ tutorial>intermediate, software-product-function>sap-ase-- bw-enablement, software-product-function>sap-ase-- erp-enablement, software-product-function>sap-ase-- hadr-enablement]
primary_tag: products>sap-adaptive-server-enterprise
---

# Fast Track to install SAP ASE 16.0 with Vagrant
<!-- description --> Speed up your Docker installation with Vagrant.

## Prerequisites
 - You should have Oracle VirtualBox installed on your machine.

## You will learn
  - How to use Vagrant

## Intro
Vagrant is a tool to shorten setup and configuration time of software environments.

>This tutorial requires about 20 minutes of working time, download and installation times may take longer.

---

### Prepare


1. **Download** and **install** [Vagrant](https://www.vagrantup.com/downloads)

    ![Image](01.png)

    >This installation requires a restart.

2. **Download** the [SAP NetWeaver ABAP Application Server](https://developers.sap.com/trials-downloads.html). Scroll down to or search for (Ctrl+F search, not the search function) 'SAP NetWeaver AS ABAP Developer Edition 7.52 SP04' and download all eleven Parts. After the download, **unpack** them all to a convenient location.

3. **Download** and **unpack** [this GitHub repository](https://github.com/sbcgua/sap-nw-abap-vagrant) to a convenient location.

    ![image](02.png)

4. **Move** the unpacked developer trial files into the "`distrib`"-folder in the unpacked local repository.

    ![image](03.png)



### Install


1. **Navigate** to the folder with the "`Vagrantfile`" and **run** the `vagrant up` command.

    >Troubleshoot: If the installation encounters some problems, `vagrant provision` is another useful command that you can run*

    ![image](04.png)

2. Wait for the installation to finish, this may take some time. Afterwards, the console will display "Installation sequence complete" along with the time that the installation took.




### Connect to your SAP system


You should now be able to connect to your SAP System through SAP Logon with the following Properties:

![image](05.png)

>You can start and stop your machine with the commands `vagrant up` and `vagrant halt`. For a full list of available commands, type `vagrant help`.






---
