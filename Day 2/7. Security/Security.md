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

