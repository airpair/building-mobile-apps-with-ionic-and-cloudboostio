We're going to build a sample social networking mobile app with [CloudBoost.io](https://www.cloudboost.io) and [Ionic Framework](http://ionicframework.com/). **We <3 Ionic** like all of you and it is an excellent hybrid mobile app development framework in which you can build apps using web technologies you already know - HTML, JS and CSS. Ionic can be a great frontend for your Mobile Apps and CloudBoost.io can be your backend and database service. For people who landed here and have no idea about what CloudBoost.io is - 
>CloudBoost is a database-service that is designed for the "next web" and that not only does data-storage, but also search, real-time and a whole lot more which helps developers like you to build much richer apps in half less time. 

Before we begin, We have checked-in all the code into GitHub, You can clone the repo from here : 

**GitHub URL :** https://github.com/CloudBoost/sample-ionic-social-network

Here's a quick demo of the app which we're going to build : 

![](https://blog.cloudboost.io/content/images/2015/06/IonicApp-1.PNG)

####Installing Ionic Framework :


To get started, Go-to http://ionicframework.com/ and

![Ionic Framework Getting Started.](https://blog.cloudboost.io/content/images/2015/06/Capture1.PNG)

Install the Ionic Command Line tools via Node Package Manager (NPM). If you do not have NodeJS installed. Please go ahead and install NodeJS first from https://nodejs.org/.

To install Ionic Command Line, type in 

`npm install -g cordova ionic` 

into your command line and NPM will take of installing Ionic and Cordova for you. 

####Building the frontend : 

######Creating a new project : 

To create a new Ionic project, Ionic has few templates to pick from. We'll choose "tabs" template from the set of templates. 

To start a new project, create a directory and type in 

`ionic start myApp tabs`

in your command line. 

After you finish installing, type in `cd myApp` in your command line to move to your app directory. 

To run your new app in the browser, type in `ionic serve` and it'll run your app on port 8100 on localhost. 

![Ionic App Running in the browser](https://blog.cloudboost.io/content/images/2015/06/ionic.PNG)


######Building Dashboard :

Now, We need to have a textbox that takes in a text which is a "Post", and display the feed on the screen. 

Navigate to `www\templates\tab-dash.html` which is your dashboard screen

Edit the HTML to add a textbox : 

	<form ng-submit="savePost(postText)">
    <label class="item item-input">
      <i class="icon ion-ios-arrow-forward placeholder-icon"></i>
      <input type="text" placeholder="Your Post" ng-model="postText">
      <button type="submit" style="position: absolute; left: -9999px; width: 1px; height: 1px;"/> 
    </label>
  </form>
    
and add a sample feed : 

	<div class="list card">
      <div class="item item-divider">11 June,2015 at 2:10 PM</div>
      <div class="item item-body">
        <div>
          Sample Post
        </div>
      </div>
    </div>
    
Your screen should now look like : 

![Dashboard Screen of the Ionic App](https://blog.cloudboost.io/content/images/2015/06/IonicApp.PNG)

Your dashboard is now ready, Next step is to integrate backend with your app. 

####Integrating your app with CloudBoost.io: 

######Before you begin :

* You need to signup with a free account on CloudBoost [here](https://www.cloudboost.io).
* You need to create a new app. Type in the name and the AppID and click create. ==Hint : AppID is just the unique name of your app on the CloudBoost Network.==

![Create a new CloudBoost App](https://blog.cloudboost.io/content/images/2015/05/createApp.PNG)

![CloudBoost app which is created](https://blog.cloudboost.io/content/images/2015/05/AppCreated.PNG)

* After you create your app, Click on "Keys" to check out your Client Key. 

==Hint: **Client Key** is used if you're using the SDK on the client and not the server. If you're using the SDK like our NodeJS SDK on the server. Please use **Master Key** instead  ==

![Your CloudBoost.io app keys.](https://blog.cloudboost.io/content/images/2015/05/AppKeys.PNG)

######Step 2 : Build a feature to save "Posts".

Here we will implement a feature to save a post in CloudBoost database. 

* Create a table in the Table Designer at CloudBoost.io
![Go to table Designer](https://blog.cloudboost.io/content/images/2015/05/Data.PNG)

![Add new table](https://blog.cloudboost.io/content/images/2015/05/addNEwTbale.PNG)

* Once your new table is created, Add two fields to it : **name** of type **text** and **post** of type **text**. It should look like the screen below : 

![Your feed table schema](https://blog.cloudboost.io/content/images/2015/05/tableSchema.PNG)

* Once a new table is created, You can start hacking with our JavaScript SDK. Download our JavaScript SDK from [here](https://docs.cloudboost.io)






. 