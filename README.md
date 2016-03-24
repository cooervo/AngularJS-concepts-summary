# AngularJS-cheatsheet

### Useful links

* [The Top 10 Mistakes AngularJS Developers Make](https://www.airpair.com/angularjs/posts/top-10-mistakes-angularjs-developers-make) 
* [Opinionated AngularJS styleguide for teams](http://toddmotto.com/opinionated-angular-js-styleguide-for-teams/) 
* [The Ultimate AngularJS Cheat Sheet - Part 1](http://www.dotnetcurry.com/angularjs/1114/angularjs-cheatsheet-beginner-developers-part1) 
* [The Ultimate AngularJS CheatSheet - Part 2 (Intermediate to Advanced Developers)](http://www.dotnetcurry.com/angularjs/1115/angularjs-cheatsheet-intermediate-advanced-developers-part2) 
* [Webstorm angular workflow](http://blog.jetbrains.com/webstorm/2014/03/angularjs-workflow-in-webstorm/) 
* [Service vs Factory] (#service-vs-factory)



#### Service vs Factory

Basically the difference between the service and factory is as follows (more info [here](http://blog.thoughtram.io/angular/2015/07/07/service-vs-factory-once-and-for-all.html)):

        app.service('myService', function() {
        
          // service is just a constructor function
          // that will be called with 'new'
        
          this.sayHello = function(name) {
             return "Hi " + name + "!";
          };
        });
        
        app.factory('myFactory', function() {
        
          // factory returns an object
          // you can run some code before
        
          return {
            sayHello : function(name) {
              return "Hi " + name + "!";
            }
          }
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

### Factory

We can use factories in a very simple way: 

     angular
      .module('az-app')
      .factory('DataModelService', function() {
      
        //no need for getters and setters since we just need to get variables and set without any complex logic
        var name = "Mateo";
        var telephone = 018007367;
        
        //this is the object that will be injected
        return Object;
      });

And just access the vars directly or set them directly:

    DataModelService.name; //gets var name "Mateo"
    DataModelService.name = "Newname"; // sets var name to "Newname"

But I can also use getters and setters to hide complex logic from controller.

    angular
      .module('az-app')
      .factory('DataModelService', function() {
      
        //This obj is necessary because factory must return an object to be injected where needed
        var Object = {
          city:"",
          name: "Oldname",
          lastName: "Lastname",
          idNumber:"007",
          telephone: "01800",
          address: "Calle 90"
        };
        
        //----setters---- 
        Object.setFullName = function(name, lastName){
          Object.name = name;
          Object.lastName = lastName;
        }
        
        //----getters----
        Object.getFullName = function(){
          return Object.name + Object.lastName;
        }
        
        //this is the object that will be injected
        return Object;
      });

This more complex way we can use **encapsulation** whenever we need complex getters and setters, for example if we need to convert a Date object to a string representation of Date this would be useful. Remember encapsulation is good as we can hide logic from the controller.

    DataModelService.setFullName("Mateo", "Cuervo");
    DataModelService.getFullName; //returns "Mateo Cuervo"

##### To use chrome remote debuggin
1. Open app in device
2. Open chrome
3. type in navegation bar: `chrome://inspect`
4. Select device to inspect
