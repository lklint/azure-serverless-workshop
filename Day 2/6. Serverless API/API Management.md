# API Management

First step is to create an instance of the API management. Go to the Azure Portal, search for *API Management* and go to the overview. 

Choose your subscription, resources group and region like the previous resources you have created. 

Choose the name for your organisation, which should normally reflect the actual company the project belongs to. Enter the admin email too (just use your own for now). 

Leave the pricing tier as *Developer (no SLA)* and click **Review + Create**. 

*Monitoring, Scale, Managed Identity, Network, and protocol settings* are not used for this demo. 

Sit back, get some popcorn and relax. API management services can take a long time to create, sometimes 20-30min. 

## Create you first API

When you finally have your API Management Service, it's time to create your first API. We are going to import an existing specification then publish it to use on our own, and in effect create a proxy service. 

In the left navigation of your API Management instance, select **APIs**.

Select the **OpenAPI** tile.

In the **Create from OpenAPI specification** window, select **Full** in the top left.

Enter the following values:

- OpenApi specification: *https://conferenceapi.azurewebsites.net?format=json*
- *Display Name, Name and Description* is filled in based on the API specification
- URL Scheme: HTTPS
- API Suffix: The identifier for the API within the API Management service. 

Select *Create* to create the API. 

We can call API operations directly from the Azure portal, which is a quick way to view and test the various endpoints in the API.

In the left navigation of your API Management instance, select **APIs** > **Demo Conference API**.

Select the **Test** tab, and then select **GetSpeakers**. The page shows **Query parameters** and **Headers**, if any. The **Ocp-Apim-Subscription-Key** is filled in automatically for the subscription key associated with this API.

Select **Send**.

## Products

A [*product*](https://docs.microsoft.com/en-us/azure/api-management/api-management-terminology#term-definitions) contains one or more APIs, a usage quota, and the terms of use. After a product is published, developers can [subscribe](https://docs.microsoft.com/en-us/azure/api-management/api-management-subscriptions) to the product and begin to use the product's APIs.

Select the *Products* menu option and then click *+ Add*. 

Fill in the *display name* and *description* as you see fit.

*Published* should be checked if you want to use the product. We want to use it.

*Requires subscription* defines is the product is open/anonymous, or if a subscription key must be used to access the product. 

Add some APIs to the product. 

## Developer Portal

Open the developer portal and get familiar with the features and functions. 

## Challenge

Create an API that proxies three of the [NASA public APIs](https://api.nasa.gov/).

Then develop and publish your developer portal website. 