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



### Save image



### Save image meta data