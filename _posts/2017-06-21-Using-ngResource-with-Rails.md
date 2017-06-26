---
layout: post
title: Using ngResource with Rails Applications
share: true
comments: true
categories: Ruby AngularJS How-To
excerpt_separator: <!--more-->
---

While Ruby on Rails is great for quick and easy app development, the synchronous requests can be a real drag -- literally. AngularJS is clean, elegant, and super fast on the client-side, with its beautiful promises and async style. What if you want the best of both worlds? Using ngResource in Angular essentially turns your Rails application into an internal API, seamlessly piggybacking your robust Rails framework... without the bulkiness of Rails dragging you down.

<!--more-->

**For this article, I'm going to assume you have an existing RoR application with AngularJS added**

## Break out of your Angular
For most use cases, when we use AngularJS with Rails, we'll usually contain the small amount of code to one single file named something like `angular.js` within `app/assets/javascripts/`. While this is fine for minimal use, we're going to be throwing most of our logic into AngularJS to speed things up.

Let's create a new directory within `javascripts/` and name it `angular`.

![angular folder](/images/angular_folder.png)

Now, we'll move our `angular.js` file inside this folder. As we create more Angular code, we'll break it out accordingly.

## Update Rails controllers
For each Rails model that we need to access in Angular, we'll want to update the associated Rails controller to output our information in JSON as well as HTML. Take a look at the example below:

```ruby
  class ThingsController < ApplicationController

    # ...

    def index
      @things = Thing.all

      respond_to do |format|
        format.html                            # HTML for your view
        format.json { render json: @things }   # We'll render data in JSON for AngularJS
      end
    end

    def show                                    
      @thing = Thing.find(params[:id])

      respond_to do |format|
        format.html                             
        format.json { render json: @thing }     
      end  
    end

    # ...

  end
```

## Add ngResource to your Angular module
Next, we'll add the ngResource CDN to our application (alternatively, you can install angular-resource via [Bower](http://bower.io)). Add the following line after your AngularJS inclusion:

```
  <%= javascript_include_tag "//ajax.googleapis.com/ajax/libs/angularjs/X.Y.Z/angular-resource.js" %>
```

Then, we need to inject the ngResource dependency into our AngularJS application module. This will be in `/app/assets/javascripts/angular/angular.js`.

```javascript
  var myApp = angular.module('myApp', ['ngResource']);  // Add 'ngResource' to your module
```

## Create your factories
Now, inside our `angular` directory, we'll create a new file called `factories.js`. Inside that file, for each model, you'll want to create a factory that will handle our CRUD functions. Routing will be based on our Rails router.

```javascript
  myApp.factory('Thing', ['$resource', function($resource){
    return $resource('/things/:id.json', {id: '@id'}, {
      update: {method: 'PUT'},
      destroy: {method: 'DELETE'}
    })
  }]);
```

**Note that only update and destroy need be defined. Get, save, and query are included in ngResource by default.**

## Using ngResource factories in your Angular controllers
Once we have our factory created, we're ready to retrieve our data and store it in our Angular controller for use in our logic. First, we'll need to inject our new factory into our Angular controller.

```javascript
  myApp.controller('thingController', ['Thing', function(Thing){

    // Logic goes here...

  }]);
```

Then, we can go ahead and use our factory to pull data into Angular for use. I've included our basic CRUD operations in the example below. For async calls, we'll use promises and callbacks to speed things up.

```javascript
  myApp.controller('thingController', ['$scope', 'Thing', function($scope, Thing){

    // Use .query() to get all Things
    function getAllThings(){
      Thing.query({},    // Params go in hash, if applicable

        // When promise is returned
        function(response){
          // Do stuff
        },

        // If an error occurs
        function(err){
          // Error handling here
        }
      );
    };

    // Use .get to get a single Thing
    function getOneThing(thingId){
      Thing.get({id: thingId},
        // Callback and error handling here
      );
    };

    // We can also use the following calls in the same way as above:
    Thing.save({});       // Thing object to be saved goes in the object hash
    Thing.update({});     // Works same as save, but performs a PUT on existing object
    Thing.destroy({});    // Calls DELETE on object specified

  }]);
```

## In closing...
Hopefully now you see how using AngularJS with ngResource can help speed up your Rails application. I tried to keep this decently high-level without going to granular, so I hope you understand the concepts. ngResource can do a lot of great things, and I love using it to keep my Rails app as async as possible. It doesn't give me a single-page app, but it definitely helps things along.
