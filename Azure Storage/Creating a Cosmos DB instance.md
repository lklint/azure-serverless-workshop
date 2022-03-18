# Creating a Cosmos DB instance

We need somewhere to store meta data about the images we are scraping. Cosmos DB is a high performance and globally distributed database that we will use to store all the data about the llama images. 

<u>Azure Portal</u>

Search for Cosmos DB and go to the section.

Click on *+ Create* to open the wizard for creating a new Cosmos DB. 

## Select API option

There are five APIs to choose from. The APIs are ways of interacting with the data that you store in Cosmos DB and it can make a big difference to choose the right one.

For this project we will use the Core (SQL) API, which is often the one Azure will recommend.

Click *Create* on the Core (SQL).

## Basics

**Project details**

- Choose subscription and the resource group we are using for this project. 

**Instance Details**

- Give the new account a name. Make it a good one. 
- Choose *location* as North Europe. 
- Capacity mode should be serverless. That is kind of why were here, right?

## Global Distribution

Leave as disabled for now.

## Networking

Set *connectivity method* to All networks for now. We will re-visit this later. 

## Backup Policy

Choose a policy that suits your backup needs. For this project we can probably choose locally-redundant to save some $$. 

## Encryption

Leave as default.

Click *Review + create*, verify your selections, then click *Create*. 

Creating a Cosmos DB can take a few minutes, so be patient. 

When it is done, go to the resource for look-see. 

<u>Azure CLI</u>

First create the Cosmos account, which is where your databases will live.

`az cosmosdb create --name llamacosmos --resource-group azureserverless `

Then create the database where we will store our containers and data. 

`az cosmosdb sql database create --account-name <account name> --resource-group azureserverless --name llamascraper`

# Creating a Cosmos container

We need a specific place to store the data in Cosmos, which is what a container is for. 

<u>Azure Portal</u>

Go to the *Quick start* if you aren't taken there automatically. Make sure *.NET* is selected as platform and click on the *Create 'Items' container* button. 

This will create a container called *Items* as well as a sample .NET app. Have a look through the application and get a sense for how to interact with Cosmos DB. 

Alright, go to the *Data Explorer* menu in your Cosmos DB account and investigate the data we have so far. 

Click on *New Container* 

- Create a new Database id: llamascraper
- Container id: images
- Leave partition key as */id* 
- Leave Unique keys empty

Click *OK* to create the new container.

Expand the treeview to see *DATA -> llamascraper -> images -> items*. 

This is where the meta data about our images will be stored.



<u>Azure CLI</u>

`az cosmosdb sql container create -g azureserverless -a llamacosmos -d llamascraper -n images --partition-key-path "/id"`

In the Azure portal, go to the *Data Explorer* menu in your Cosmos DB account and expand the treeview to see *DATA -> llamascraper -> images -> items*.