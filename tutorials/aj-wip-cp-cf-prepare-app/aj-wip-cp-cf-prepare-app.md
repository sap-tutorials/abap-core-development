---
parser: v2
auto_validation: true
time: 10
tags: [ tutorial>beginner, software-product>sap-business-technology-platform]
primary_tag: software-product>sap-business-technology-platform
---

# Download and Prepare App for Cloud Foundry Development
<!-- description --> Download files for a simple app, maintain the deployment descriptor, and bundle everything together ready for deployment.

## Prerequisites
## You will learn
  - What a typical small Node.js app looks like
  - What a Cloud Foundry deployment descriptor contains

## Intro
In this tutorial, you'll download a simple Node.js app from GitHub that returns an index page with various information about itself, in response to an HTTP request. You'll open up and edit the app's deployment descriptor, a file containing information about the app, including memory requirements, hostname and more.

---

### Find the sample app of Github

The sample app is available in a repository on GitHub.

Open it up at this URL: <https://github.com/SAP/cf-sample-app-nodejs>.



### Download sample files

This tutorial preparing the app for deployment to the SAP Cloud Platform Cloud Foundry environment, and you'll be deploying it (in another tutorial) from your own machine. So you'll need to download the repository first.

Choose **Clone or download** and then choose **Download ZIP**

![Github Download ZIP](step-2.png)

Enter the name of the file you downloaded and choose **Submit Answer**.




### Extract sample files

Open the zip file and extract its content to a folder on your local computer. You will have extracted a folder named `cf-sample-app-nodejs-master`.

![Github Download ZIP](step-3.png)



### Open the deployment descriptor

The sample app repository contains a basic deployment descriptor, but you'll need to edit it before the deployment. The deployment descriptor is the file called `manifest.yml` in the root of the app folder structure.

Open the `manifest.yml` file with an editor of your choice.

What is the value of the `name` parameter that exists already in the `manifest.yml`?




### Add a unique host name to the deployment descriptor

The host name for your app must be unique within a combination of region and environment, in that it forms the most significant part of the fully qualified hostname in the app's URL space. For example, if your app host name is `myapp`, the fully qualified hostname for the Cloud Foundry environment in the `EU10` region will be:

```
myapp.cfapps.eu10.hana.ondemand.com
```

If someone else is already using that hostname within that region and environment, you won't be able to deploy your application.

1. Add a parameter named `host` to the `manifest.yml` file and provide a unique value. For example, use your birth date or a random number or string or incorporate your user ID:

    ![Github Download ZIP](step-5.png)

    > The YAML format uses whitespace significantly – make sure you format this new parameter correctly, as shown.

1. Don't forget to save your changes.



---
