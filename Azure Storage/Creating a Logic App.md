# Creating a Logic App

<u>Azure Portal</u>

Go to the Logic Apps section in the Azure Portal. 

Click *+Add* to start the Logic App wizard experience. 

## Basics

Choose subscription and resource group.

Give the Logic App a name, such as *llamaimagescraper*, which is unique across all of Azure. 

Set the plan type to *consumption*, which will reload part of the form. 

Choose a region such as North Europe. 

Click *Review + create* to create the Logic App. 

## Designing a Logic App

In order for the Logic App to *do* anything, we need to add, well, logic. This can be done in a few ways, and we start with the visual way. 

Go to the *Logic app designer* sidebar menu, which will open the designer for your logic app. You will see the start screen which has a video on what Logic Apps do and then several triggers and templates. 

### Trigger

Every Logic App needs to start with a trigger, and we will use the *When a new tweet is posted* trigger. Click on it and the designer will open.

Connect the trigger to an active Twitter account. It doesn't matter which one, and you can create a new one if you want to. Click on *Sign in* on the Twitter step in the designer and log in to your account. This will create an authorized connection between the logic app and Twitter, so we can read tweets and get some llama images (hopefully). 

We now have to configure the Tweet trigger so we get the right data out.

- Search text: We are going to use *llama* to get tweets that contain that specific string.
- Choose a frequency that make sense, such as every minute, at least when we are testing the logic app

### Condition

The whole idea of scraping for llama images is of course to get some actual images. We are making a bold claim that tweets containing the word *llama* might also have llama images, but we are definitely only interested in tweets that has images attached. 

To do this, we use a *condition*. Press the *+ New Step* button underneath the Twitter trigger. This opens up the dialog for adding a new step. Kinda obvious, I know. Search for *control* and then click on the *control* action, and then on *condition*. You can now choose a condition, which evaluates to true or false. 

We want to evaluate whether there is any media with the tweet or not. Click on the box to the left of the dropdown for the evaluators (contains, greater than, etc.) and another dialog pops up. Are you with me? Go to the *Expression* tab of that dialog and choose *length*. We want to make sure the array of media items has more than 0 objects. Click inside the *length* function, meaning inside the parenthesis, and then choose the *dynamic content*  tab, where you find the *Media urls* field. You should end up with a function looking like this

`length(triggerBody()?['MediaUrls'])`

Then choose *is not equal to* in the dropdown field. Finally, add "0" to the right hand side of the condition, so it reads 

`length(..) is not equal to 0`

Are you with me so far? I should have added some screenshots. Apologies. 

### Action

We now have a tweet triggering our logic app flow, and we have a condition to make sure there is one or more pieces of media (pictures mostly) attached to the tweet. What do we do with it? We need some action! 

The condition step has created a *true* path and a *false* path for us. Let's fill out the false path first, as that is quick. What do we do if the tweet doesn't have any media? Nothing. Add an action to the *False* path, search again for *control*, but this time choose the *Terminate* action. Set the status as *cancelled*. We don't need more information than that.

Back to the *True* path, which is where we get to the good stuff. Because we can have more than one media item attached to a single tweet, we need a way to iterate through them all. Inside the *True* path add an action and once again use the *control* action, but this time use the *for each* option. This will prompt you to choose which output from the previous step to iterate over, which of course is the dynamic field *media urls*. 

We need three more actions to store the media and the meta data for the media: Get image, save image, save image data. 

### Get image

We know the url of the image to get, but we don't have the actual file. Yet. We have to make an HTTP request to get the binary stream for the file. Add another action, this time the *HTTP* action. While this action can build up a full HTTP query with parameters, cookies, a custom body and more, we are only going to use the method and URI.

Method: GET

URI: Dynamic field *Current Item* 

That is it. That will request the tweet image using the URI coming from the tweet's media values. 

### Save image

Now we get to using the blob storage that we created earlier in the workshop. Finally, right? We will use the stream returned from the HTTP request and turn it into a blob in the Azure Storage container. 

Add an action, and search for *blob*. I mean that in itself is excellent. Blob. Anyway, choose the *Create block blob* action. Then configure it like this:

**Storage account name**: Use the dropdown to choose a connection to the existing storage account we created earlier. 

**Path to upload**: This is the path to the container we created such as `/llamaimages`. Don't forget to add the forward slash `/` to make it a qualified path. 

**Blob name:** This should be unique and can be made up of any of the values from the previous steps and standard text. In this case you could use 

​	the *user name* of the tweet `+` *Original tweet id*`.jpg` 

Blob content: This is where we catch the stream from the previous HTTP request. Use the *binary* expression function and add the dynamic content from the *HTTP body*. You should end up with a value like

`binary(body('HTTP'))`

That now creates an image blob and saves it to our storage account. Neat!

### Save image meta data

We also want to save a bunch of data about the image we are collecting. This meta data will be stored in the Cosmos DB database we created earlier. 

Add an action after the *Create block blob* action in the Logic App, and search for *Cosmos*. Add the Azure Cosmos DB action, and choose the *Create or update record* option. Then fill in the connection details for Cosmos:

**Connection name**: Give the connection a name, such as `llamascrapermetadata`. 

**Authentication type**: Choose *Access Key*. 

**Account ID**: This is the name of the Cosmod DB account.

**Access Key**: Go to the Cosmos DB account (in a separate browser tab) and then go to *Keys* and copy the *Primary Key* value. This is super secret, so don't share it with anyone. It gives anyone access to your data in Cosmos DB.

Then click *Create*. 

When the connection to Cosmos DB is created, it is time to insert data, or a document as it is called. Cosmos DB is a document database, which also means you can insert data in pretty much any format.

For *account name*, *Database ID*, and *Collection ID* us the dropdowns to choose the values corresponding to where you want to store the meta data. 

The *Document* is where we will store the meta data for the Tweet image. It is in JSON format, and will look something like this:

`{`

​	`"blob": "/containername/<tweet user name>+<tweet tweet id>.jpg",`

​	`"id": "<tweet tweet id>",`

​	`"tweettime": ''<tweet created at>",`

​	`"tweeturl": "https://twitter.com/<tweet user name>/status/<tweet Tweet id>"`

`}`

Where the `<tweet>` fields are dynamic fields in your Logic app. 

## Challenges

There are at least two things that could fail with this setup.

- *Llama* is Spanish for "flame", so tweets in Spanish might not talk about llamas. How do you fix this?

- If there is more than one media file for the tweet, there will be an id clash causing the document insert to fail. How would you fix this?

- We are collecting all and any kind of images. There is no filter. What could be a solution to this?

## Bonus Task

Now that you have been through the whole thing in the Azure Portal, I should probably tell you that you can use Visual Studio code with Logic Apps too. 

In VS Code, install the extension *Azure Logic Apps (Consumption)*. If you haven't already, also install the *Azure Account* extension, so you can access your Azure account through VS Code. 

Open up your Logic App in VS Code and browse the JSON, which defines it. See if you can connect the code to the visual designer. 