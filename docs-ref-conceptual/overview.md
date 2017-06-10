---
title: Azure libraries for Node.js
description: Overview of the Azure management and service libraries for Node.js
keywords: Azure, Node.js, SDK, API, management , client, services
author: TomArcher
ms.author: tarcher
manager: douge
ms.date: 06/09/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: nodejs
ms.service: multiple
---

# Azure libraries for Node.js

The Azure libraries for Node.js help you manage Azure resources and connect to services from your applications. The libraries are available as [npm packages](node-sdk-azure-install.md) for use in your projects. 

## Manage Azure resources

Create and manage Azure resources from your Node.js applications using the [Azure SDK for Node.js](java-sdk-azure-get-started.md). Use these libraries to create and query resources from your apps or to build your own Azure automation tools. 

For example, to create a Linux VM using an existing network interface, you would write the following code:

```javascript
const msRestAzure = require('ms-rest-azure');
const ComputeManagementClient = require('azure-arm-compute');

// read in service principal values from env variables
const clientId = process.env['CLIENT_ID'];
const domain = process.env['DOMAIN'];
const secret = process.env['APPLICATION_SECRET'];
const subscriptionId = process.env['AZURE_SUBSCRIPTION_ID'];

msRestAzure.loginWithServicePrincipalSecret(clientId, secret, domain, function (err, credentials, subscriptions) {
    if (err) return console.log(err);
    const computeClient = new ComputeManagementClient(credentials, subscriptionId);
    // customize the VM 
    const vmParameters = {
        location: "eastus",
        osProfile: {
            computerName: "newLinuxVM",
            adminUsername: adminUsername,
            adminPassword: adminPassword
        },
        linuxConfiguration: {
            ssh: {
                publicKeys: [mySshKey]
            }
        },
        hardwareProfile: {
            vmSize: 'Basic_A1'
        },
        networkProfile: {
            networkInterfaces: [
                {
                    id: myNetworkInterfaceId,
                    primary: true
                }
            ]
        },
        storageProfile: {
            imageReference: {
                publisher: 'Canonical',
                offer: 'UbuntuServer',
                sku: '16.04-LTS',
                version: 'latest'
            },
        }
    };
 
    // create the VM
    computeClient.virtualMachines.createOrUpdate("myResourceGroup", "newLinuxVM", vmParameters, function (err, data) {
        if (err) return console.log(err);
    });

});
```

Review the [install instructions](node-sdk-azure-install.md) for a full list of the packages and the [get started article](node-sdk-azure-get-started.md) to set up authentication and run sample code to create and update resources against your own Azure subscription. 

## Connect to Azure services

In addition to using the Azure packages to create and manage resources within Azure, you can also use packages to connect and use Azure cloud services in your apps. For example, you might update a table SQL Database or upload files to Azure Storage. Select the package you need for a particular service from the [complete list](node-sdk-azure-install.md) and visit the [Node.js developer center](https://azure.microsoft.com/develop/nodejs/) for tutorials and sample code to learn how to use the libraries in your apps.

For example, to print out the contents of every blob in an Azure storage container:

```javascript
var azure = require('azure-storage');
var blobService = azure.createBlobService(storageConnectionString);
blobService.listBlobsSegmented('testcontainer', null, function(error, result, response) {
   if (err) return console.log(err);
   console.log(result);
});
```

## Sample code and reference

The following samples cover common tasks with the Azure management packages and have code ready to use in your own apps:

- [Virtual machines](node-sdk-azure-virtual-machine-samples.md)
- [Web apps](node-sdk-azure-web-apps-samples.md)
- [SQL Database](node-sdk-azure-sql-database-samples.md)
   
A [reference](https://docs.microsoft.com/nodejs/api) is available for all packages in both the service and management libraries. New features, breaking changes, and migration instructions from previous versions are available in the [release notes](node-sdk-azure-release-notes.md).