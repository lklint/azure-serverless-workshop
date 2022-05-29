# Azure Tooling

There are some tools which make your Azure developer journey more enjoyable. Here we look at some that make local development more accessible. 

## Storage Explorer

The storage explorer lets you manage and access your data on Azure Storage accounts, locally. This is much easier than having to go through the Azure Portal every time you need to check, download, analyse or just admire your data. 

Go to the [Azure Storage Explorer site](https://azure.microsoft.com/en-us/features/storage-explorer/) to download the latest version.

Install the Storage Explorer on your local machine, and open it. It's pretty. 

Choose the Azure accounts and subscriptions you want to view in the Explorer. 

Open up the storage account we have created in this workshop. See if you can find the data we have collected. 

Notice there is a *local & attached* section in the *Explorer panel*. This is where you can create storage accounts, including blob storages and file shares that are only stored locally. 

### Storage emulator

You can use an emulator on your local machine to completely eliminate the need for cloud accessed data when developing your application. 

Currently, the recommended emulator is Azurite, which is open source and available [here](https://docs.microsoft.com/en-au/azure/storage/common/storage-use-azurite?tabs=visual-studio). 

If you have Visual Studio 2022, it comes by default with it, and you can use the *Storage Emulator* option for Azure Function projects, ASP.NET projects and more. 

If you have time, either install Azurite or get familiar with it. 

## Azure Cosmos DB Emulator

"The Azure Cosmos DB Emulator provides a local environment that emulates the Azure Cosmos DB service for development purposes. Using the Azure Cosmos DB Emulator, you can develop and test your application locally, without creating an Azure subscription or incurring any costs." - Microsoft

[Download the Cosmos DB emulator](https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator?tabs=ssl-netstd21) and install it. 

Launch the emulator and your favourite internet browser should open. 

Replace the previous connection string to your Azure Cosmos DB instance with the local emulator connection string in your Azure Function. Then run the application again to verify data is being inserted into the emulator. 