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

Navigate to `www\templates\tab-dash.html` which is your dashboard screen. 

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


######Step 3 : Integrate CloudBoost.io with your Ionic App.

In the `<head>` section of `index.html`, link the CloudBoost JavaScript SDK. 

`<script src="https://cloudboost.io/js-sdk/1.0.0.js"></script>`

*You can download the SDK and link it locally into your app too. It's a better option since SDK will not load with every app launch.*

Once you've linked the SDK. You need to Initialize your new CloudBoost App. In the `js` folder you'll find the file called  *app.js* and You can add this code in the `run` function of angular inside of that file which will initialize your new CloudBoost App. 

`var appId="YOUR APP ID"; //This should be your AppID`

`var appKey="YOUR CLIENT KEY"; //this should be your AppKey`

Once this is done, You can now call CB.CloudApp.init() function which will init your app. 

`CB.CloudApp.init(appId,appKey);`

######Step 4 : Saving Post

To save a post you need to navigate to controllers file in js directory. Look for `DashCtrl` and have the save function to save the post 

	$scope.savePost = function(text){
      var post = new CB.CloudObject('feed');
      post.set('text',text);
      post.save({
        success : function (post){
          //success
        },error : function(error){
          var myPopup = $ionicPopup.show({
            title: 'Oops! We cant save the post right now.',
            scope: $scope,
            buttons: [
              { text: 'Ok' }
            ]
          });
        }
      });
    }
    
Here you create the new `CloudObject` of type `feed` (TableName) and call the `save` function of CloudObject to save the new feed to the table. 

######Step 5 : Show the list of posts on dashboard

To see the list of posts when you launch the app, You need to create an angular `init` function and query posts from the feed table. 

	  $scope.init = function(){
        var query = new CB.CloudQuery('feed');
        query.orderByDesc('createdBy');
        query.find({
          success : function(list){
              //list is an array of CloudObject. 
              $scope.posts = list;
              
          }, error : function(error){
             var myPopup = $ionicPopup.show({
              title: 'Oops! We cannot get the list of posts right now.',
              scope: $scope,
              buttons: [
                { text: 'Ok' }
              ]
            });
          }
        });
    }
    
Now you need to bing the `posts` array to HTML by using Angular ngRepeat. 

	<div class="list card" ng-repeat="post in posts">
      <div class="item item-divider">{{post.createdAt}}</div>
      <div class="item item-body">
        <div>
          {{post.get('text')}}
        </div>
      </div>
    </div>
    
    
 
    
######Step 6 : Make your app real-time.

To refresh the app automatically when someone anywhere in the world add's a new "Post" to the feed table, you need to implement the real-time features of CloudBoost.io. Implementing real-time with CloudBoost.io just few lines of code. You need to listen to `on` event of CloudObject class. 

	  CB.CloudObject.on('feed','created', function(post){
        $scope.posts = [post].concat($scope.posts);
      });
    
Place the above code in the "init" function of dashboard controller. 

####You're done! 

Congratulations on creating your first Ionic App with CloudBoost.io. 

Here's a quick demo of your new app : 

![](https://blog.cloudboost.io/content/images/2015/06/IonicApp-1.PNG)


We have checked all the code into GitHub. You can download the code and if you're adding new features in, Feel free to send us a pull-request. ;) 

**GitHub URL :** https://github.com/CloudBoost/sample-ionic-social-network

Let us know if you've liked the small quickstart and yes, Share the love. 


