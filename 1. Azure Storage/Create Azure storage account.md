# Create Azure storage account

To have somewhere to store the data we collect (images), we need a new storage account. 

<u>Azure Portal</u>

Go to *Storage accounts*, then create a new account.

## Basic 

**Project details**

- Choose your subscription
- Create a new resource group, named something like *azureserverless*

**Instance details**

- Give the new storage account a unique name. It has to be unique across all of Azure.
- Choose your preferred region, such as *North Europe*
- Choose *Standard* performance tier, which is fine for this project.
- Choose locally redundant storage (LRS), to save costs.

## Advanced

Read through the options and leave all as default.

## Networking

**Network connectivity**

- Leave the public endpoint for all networks. This is the least secure, but we can always change it later.

**Network routing**

- Choose your preferred option

## Data protection

Read through the options and leave all as default.

## Encryption

Read through the options and leave all as default.

Click *Review + create*, confirm the settings, then click create



<u>Azure CLI</u>

Create the resource group first. This is where all our resources for the entire project will live.

`az group create --name serverlessRG --location northeurope`

Then we can create the storage account inside the resource group.

`az storage account create -n <unique name> -g serverlessRG -l northeurope --sku Standard_LRS`

# Create blob container

The storage account doesn't hold any data in itself. We will need to create a container for that first. 

<u>Azure Portal</u>

Go to the storage account and click on the *Containers* menu, then click on the *+ Container* button at the top.

- Name: pick a name for the blob container
- Public access level: Blob (anonymous read for blobs)

Click *Create*.

Open the blob container by clicking on it. Notice the various menus and 

<u>Azure CLI</u>

`az storage container create --name llamaimages --account-name <storage account name> --auth-mode login`

### Upload file

Try and upload a file to the blob container using the Azure Portal. 