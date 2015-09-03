# AngularJS-cheatsheet

### Directives

A good way to understand directives is by way of analogy. Think for a moment about how web pages are created: we write HTML tags and each tag defines a different behavior. When we use `<ul>` and `<li>` tags, we’re telling the browser we want an unordered list. When we use HTML5 `<video>` or `<audio>` tags, we’re telling the browser we want a video or audio track to be displayed.

But there are only a finite number of HTML tags that the browser knows how to interpret. What if we wanted a `<weather>` tag to show the weather? What about a `<login>` tag to show our user login panel? Directives give us the power to create our own HTML elements. That is, directives let us specify custom DOM elements (or attributes) and attach behaviors to them.

A directive is simply a function that we run on a DOM element to give it added functionality. In Angular, we’ll use directives everywhere.

For instance, we can use the ngClick directive to tell an element to run a function when it’s clicked. For instance:

    <button ng-click="runWhenButtonClicked()">Click me</button>
    <!-- Or another element -->
    <div ng-click="runWhenDivClicked()">No click me instead</div>

In the example above, the ng-click HTML attribute is a directive that adds an event listener to the click event to the <button> and the <div>. There are a lot of built-in directives provided by Angular.


### Modules

Once we get to our javascript code, how do we tell Angular where our angular app is? We need to define it using the angular.module() API. There are two main operations when dealing with a module:

creating a new module (i.e. the “setter”) or getting a reference to an existing module (i.e. the “getter”)

When we want to create a module we use the **setter** arguments:

    angular.module('myApp', []);

When we want to get an existing module we use the **getter** arguments (i.e. no arguments):

    angular.module('myApp');
    
* The only difference between the two is that the setter takes an array as the second argument. 

Once we’ve created the module `myApp` , we can assign the app to a particular place in our code by using the ng-app directive. For instance, if we have the following html:

    <html ng-app="myApp">
      <head>
        <!-- etc. -->
      </head>
      <body>
        <!-- etc. -->
      </body>
    </html>

`myApp` is essentialy the “main” module for the page and the app will be automatically started (also known as bootstrapped) for us when the page is done rendering.

### Scopes (Angular has many scopes)

A powerful (but potentially confusing) feature of Angular is that you can have many scopes in your Angular application. The `$rootScope` is the top level scope for the rest of our application.

That means that anywhere in the view (i.e. all children elements under the DOM element with the `ngApp` directive) we can reference variables that are on the `$rootScope` object.

However, if we overuse this one `$rootScope` object, we could end up putting a lot of information on a single scope. If we put every variable on a single scope, then we don’t have any advantage over just using global variables; in a large, complex application we’ll end up having collisions with data on the scope and the code can easily get confusing. To prevent this, Angular provides the ability to organize scopes by using parent/child relationships.

Just like our DOM elements are nested in each other, scope objects can be nested. In the same way the HTML tags can have a parent, scopes can have a parent. When Angular looks up the value of a variable it will look at the current scope and then look “upwards” for the variable in any of the parent scopes.

Angular creates scopes in a variety of situations. For instance, a child scope is created for every controller in our app.

But before we move on to controllers, we want to point out something that might not be obvious at first glance: a scope is a plain old javascript object (also known as a POJO). Although a scope does have functionality that makes it really useful, it isn’t magic. Scopes are javascript objects just like everything else in our program.

Summary: **scopes are a way to organize key-value pairs without polluting a global namespace.**

### Controllers

A controller is a bit of code that defines functionality for a part of the page.

Take a look at this example:

    angular.module('myApp')
    .controller('HomeController', function($scope) {
      // We have access to this new 
      // $scope object where we can place
      // data and functions to interact with it
    });
    
Now we have a controller defined, we can place it on the page using the ngController directive, like so:

    <div ng-controller='HomeController'>
      <!-- 
        In here, we have access to the
        $scope object defined by the HomeController
       -->
    </div>
    
Now, instead of placing all of our functionality on the special `$rootScope` object, we place can place it on the HomeController’s `$scope` object and keep our `$rootScope` clean.

The benefit of creating a new scope is that we’re able to keep variables and data contained to this specific part of the page. We can define a new variable without it polluting or conflicting with another part of the page.

Technically, every time that we create a new controller, Angular creates a new `$scope` object underneath the parent scope. (In this case the parent is `$rootScope`.)

### $http, XHR, and Promises

Once we understand how to interact with data from within our own application, then the next logical step is to get data from the outside world and use it in our application.

How can we interact with back-end APIs with Angular?

Angular comes bundled with a wrapper around the **XMLHttpRequest** (also known as **XHR**, which is how you perform **AJAX**) called `$http`. The `$http` object is a library that helps you make HTTP requests and then parse the response. We often use `$http`

    $http({
      method: 'GET',
      url: 'http://foo.com/v1/api',
      params: {
        api_key: 'abc'
      }
    });

When this method runs, it will go and make a `GET` request to the back-end server at `http://foo.com/v1/api` with the parameter of `api_key=abc`.

`XHR` requests are **asynchronous**. This means that our application doesn’t have to pause our application and wait for a response from the server. The benefit is that the user can continue to use our application while the HTTP request is being made, but it introduces problems because once the data does come back from the server we have to deal with it. Because of this, asynchronous control flow it’s easy for our code to become cumbersome without the right tools.

In general, we have two options:

Pass a callback function. This is a function that will be called when the HTTP request completes.
Use a promise. This is the approach Angular takes.



### Promises

Promises are objects that help make working with async code feel like we’re writing synchronous code. Angular uses promises extensively, so it is important to get familiar with how to use them.

We use primarily only three methods when we use promises:

    promise
    .then(function(data) {
      // Called when no errors have occurred with data
    })
    .catch(function(err) {
      // Called when an error has occurred
    })
    .finally(function(data) {
      // Called always, regardless of the output result
    })
    
The $http object returns a promise when it’s completed the XHR request. Now, to interact with our request, we’ll simply use the .then() function to load the data on our $scope:

    var promise = $http({
      method: 'GET',
      url: '/v1/api',
      params: {
        api_key: 'abc'
      }
    });
    
    promise.then(function(obj) {
      // obj is the raw request object generated by Angular
      // and contains status codes, the raw data, headers,
      // and the config function used to make the request
      $scope.data = obj.data;
    });
    
### Dependency Injection

**Depdendency Injection (DI)** is a term for how code gets a reference to it’s dependencies. Dependency injection, like use `require` in Node.js, `require` in Ruby, or `import` in **Java** refers to how objects get access to the dependencies they need to run properly. For instance, we’re not going to write the printf() function in C because we can us the `libc` library that has it implemented already. This `libc` library is considered a dependency.

“Dependency injection” refers the process of us telling Angular what dependencies we need to use and Angular resolving dependencies when we need them.

For instance, in the following controller, we’ll want to get access to both the $scope object and the $q service. We’re telling (i.e. annotating) what we need to run our controller. Then, at runtime Angular will handle passing in (injecting) the dependencies for us.

    angular.module('myApp', [])
    .controller('HomeController', function($scope, $q) {
      // We have the $scope object and the $q object
      // available in here
    });
    
### Services

Services are another core concept in AngularJS. When we used $http we were using an Angular service. Services are singleton objects that perform tasks common to several areas of the system.

Take the `$http` service for example: performing HTTP requests doesn’t belong to a specific controller. We need to to make HTTP requests in lots of places in our code.

If we want to set cookies in the user’s browser, that is a task that doesn’t belong to a specific controller either. To set cookies, we will use angular’s $cookies service.

Say we were writing a weather app. We might write our own WeatherService which would be common code that we could use to get the weather at any point in our application.

    angular.module('myApp.services', [])
    .service('WeatherService', 
      function($http) {
        this.weatherFor = function(zip) {
        // do something with $http to get the weather here  
      };
      });
      
Then we could use our weather service in our controller:

    angular.module('myApp.controllers')
    .controller('WeatherController', function($scope, WeatherService) {
      $scope.weather = WeatherService.weatherFor(90210);
    });

To reiterate: services are only created once. There is only a single instance of a given service in your application.

The idea here is that you want to keep your controllers thin. In the example above we technically could have written weatherFor in the controller directly, but by pulling it into a service we isolate responsibilities (e.g. each section of code does one thing). Also by having weatherFor in a service, that functionality is available to other controllers.

When we find our controllers getting bloated, it’s time to try to move code out of our controller and into a service. Not only is this good programming practice, it is easier to test services as a unit when they aren’t mixed up with the rest of our controller code.

* What’s the difference between a **service**, a **factory**, and a **provider**? As you read more Angular code you’ll see these three terms used almost interchangeably. That’s because *they’re all the same thing. service and factory are both implemented by provider under the hood. The difference is in the level of configuration you have when creating each one. 

### Useful ionic commands

Create a local server that updates the app in the **browser** as changes are saved.
  
      ionic serve 
      
Updates saved changed in the app in the **android device** VERY USEFUL!!!
  
      ionic run android -l -c

### Run chrome as a non secure instance for VPN and ajax

use this commmand inside the desktop link (acceso directo) of chrome

    "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --args --disable-web-security

