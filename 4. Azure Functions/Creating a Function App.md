# Creating a Function App

An Azure function lives inside a Function App, so that is the first thing we need to create. 

<u>Azure Portal</u>

Search for Azure Functions in the portal and go to the *Functions App* section. 

Click on the *+ Create* button in the top left to create a new Function App. 

## Basics

Choose your subscription and the resource group we created earlier. 

Give the Function app a name that is unique across Azure, such as *llamawebsite* (which is probably already taken). Wait for the green tick to confirm you can have that name.

Choose *Code* as publish method. 

Runtime stack is *.NET*, but you can also use *Node.js, Python, Java*, and more.

.NET 6 is our preferred version. That is the latest LTS version.

Set the region as *North Europe*.

## Hosting

The Function App needs a valid storage account where meta data about the Function App is stored. Choose the one you created earlier. 

The operating system is *Windows*, but .NET 6 can also run on *Linux* if you prefer. 

For the plan type there are three options

- *Consumption*, which is the original serverless model. This means compute on Azure is only allocated when you need it, i.e. the function is running.
- *Premium*, which fires up one more VM instance than you need to run your workload. This eliminates the dreaded *cold start*, which is when a VM fires up for the first time to serve your function request.
- *App service plan*, which most always forget about. If you already have an App Service Plan hosting other App services and more, then you can use it to run your functions too. You are already paying for the App Service plan, so it can be cost effective. 

Choose *consumption*, as this workshop is all about serverless. 

Then click *Review + Create*, as we aren't using neither *Networking* nor *Monitoring* this time around. 

Click *Create* when you confirm your choices. 



<u>Azure CLI</u>

If you choose to use the CLI to create your Function App, use this command. 

`az functionapp create -g azureserverless -n <function app name> -s <storage account name> -c northeurope --os-type windows --runtime dotnet `



# Azure Function

Go to the Function App in the Azure Portal and have a look around. You can see the Function App even has an App Service Plan, even though we chose the *consumption* model. This shows again, that serverless is just using someone else's server. 

Of course we need to have an actual function to do anything. Use the *Functions* menu to go to the list of functions and then click *+ Create*. This will open the blade to create a new function. 

The first thing you will see is *Development environment*. I promise we will use Visual Studio Code in just a minute, but for now choose *Develop in portal*. 

There are then a bunch of templates we can choose from. Have a scroll through them. We are going to use the *HTTP trigger* template. Click it. 

Give the function a name, such as *testtrigger*. We aren't going to use this in our project, so this can be anything. This function is to get an understanding of how they work first. 

The *authorization level* dictates how much you treasure the function's....well, function. There are three choices

- *Anonymous*: Anyone with the right URL can call the function. This could be dangerous, if the function either manipulates some data, or serves up some valuable data. 
- *Function*: This means you need the function level key to access the function. There is a key per function in the Function App.
- *Admin*: You need the master key to access the function. 

Choose anonymous for now, as this is just a test. Hardly a top secret plan or business secrets. 

Click *Create*, then admire your new wonder of the serverless world. Go to the *Integration* menu and see that the *trigger* is and HTTP request, which feeds into our function. Click on the trigger and an edit panel will open. The main new property is the route template, which allows you to specify a relative URI path for the function to be triggered on, such as `/neworder/{orderid}`. You can create a basic API like this.

Close the edit trigger panel.

Click on the name of your function. This opens the code editor window in the portal. Let's investigate the function template that was created for us.

Click the *Test/Run* button above the code. This will open a panel with the test data to provide to the function. By default it is set to *POST* and a prefilled *Body* section too. 

Click *Run* and the function will compile, spin up a VM to host the function, run it, and then report back.

Click on *Get function URL*, which will generate the URL to call the function externally from the Internet. Notice there is no function key attached. Go back to the *integration* menu, click on the trigger again and then change the *Authorization level* to *Function*. 

Click *Save* and open the function code again. Now, get the function URL again and notice the `code` query parameter. 

# Visual Studio Code

While you *can* develop your function in the Azure Portal (and there is nothing wrong with that to start with), I prefer to use an IDE like VS Code. 

## Extensions

In VS Code install the *Azure Functions* extension. This will also install the *Azure Functions Core Tools*, which "lets you develop and test your functions on your local computer from the command prompt or terminal." 

Also, install the *Azure Databases* extension, which will let you see the Cosmos DB database inside VS Code.

You should now have a *Functions* section in your Azure section of VS Code. This will connect to your Azure account and show any existing Functions, like the one we just created. However, functions created in the Azure Portal are read only in VS Code. This is to avoid unintentional changes that will create conflicts. 

## Local Function

Instead we will create a local function app that we can push to Azure. In the *Functions* section in VS Code click on the little icon for *Create New Project...* and choose a local folder on your machine to create it in.

Then go through the wizard to create a new function project:

C# -> .NET 6 -> Http trigger -> Choose namespace -> Create function

Read through the code, and then open the *local.settings.json* local file to see the settings for your Function App. 

Hit F5 to build and run the function on your local machine. In the *Terminal* window, there should be something similar to this output

`HttpCosmos: [GET,POST] http://localhost:7071/api/HttpCosmos`

where `HttpCosmos` is the name you gave the function. The Azure Function runtime is now running on your local machine, hosting the function we just created on that `localhost` address. Copy paste it into your browser. Give it a bash. You can also right-click on the function name in the Azure menu in VS Code and choose *Execute function now*. 

It is the same function as we created in the portal (it is a template after all), but now it is on your local machine, with no need for Azure (yet). 

## Push to Azure

You should already be signed into Azure in VS Code, but if not, do it now in the left-hand Azure menu.

In your *Functions* section in the Azure menu, press the deploy icon, which looks like an upward arrow going into a cloud. Select the folder and subscription to use. Then select *Create new Function App advanced*. Choose a new unique name for the function app, .NET, Windows, our existing resource group (*azureserverless*), North Europe, Consumption, the existing storage account, skip app insight resource, and then let the new Function App come to life.

We are creating a new Function App so you can see the experience from VS Code. Again, you will get a URL to trigger the function from the Output window. Give it a go. 

## Connecting to Cosmos DB

To connect to your Azure Cosmos DB account, you must add its connection string to your app settings. You then download the new setting to your *local.settings.json* file so you can connect to your Azure Cosmos DB account when running locally.

