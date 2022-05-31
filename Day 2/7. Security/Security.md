# Security

Security is built in to every part of Azure, yet there are still lots of things that user can mess up. This module looks at two of the most commonly used ways to add an extra layer of security for authenticating and using Azure resources. 

## Managed Identity

Managed identities provide an identity for applications to use when connecting to resources that support Azure Active Directory (Azure AD) authentication. 

We are going to update the Logic App we built earlier in the workshop to use managed identity to authenticate with the storage account. 

Open the Logic App you created earlier. Go to the *Identity* menu and notice there are two types of identities you can assign. 

Choose the system assigned and set it to *on*.

Click *Save*.

Go to the Storage account you created earlier and find the *Access Control (IAM)* menu. 

Click *+ Add* and then select the *contributor* role. Click *Next*.

Select *Managed identity* and then select the members to add, which is the Logic App managed identity. 

Click *Next* and and *Review + assign*

Now go back to the Logic App and change the authentication for the storage account to managed identity. 

Do the same for the Cosmos DB connection.

## Azure Key Vault

We will use a Key Vault to store the connection string for Cosmos DB, then retrieve it from there.

Search for *Key Vault* in the Portal and go to the resource section. 

Click *+ Create* to create a new Key Vault.

Fill in the Basic information, which by now should be familiar. Leave pricing tier as *Standard*.

Click *Next* and set the permission model to *Azure role-based access control*, also known as RBAC.

Click *Next* and leave connectivity for all networks. 

Click *Rewiew + Create* then create the Key Vault.

Assign your Azure user the permission of *KeyVault Administrator* to allow you to insert values/secrets/keys into the new vault. 

Enable the system assigned managed identity for the Function App we created earlier.

Assign the *Reader* role to the Function App in your new Key Vault app. 

Create a new secret in the Key Vault and store the connection string from your Cosmos DB with name *CosmosDbConnectionString*. 

Click on the new secret to open up its settings and copy the URL to the secret. 

In your Function app, go to the *Configuration* tab and add a new Application Setting. Call it *CosmosDbConnectionString* and give it the value of 

`@Microsoft.KeyVault(SecretUri=<the secret's URL>)` 

### Challenge

Change the Azure Function code to use the Environment variable instead of the static config string

`ConnectionStringSetting = "CosmosDbConnectionString"`

