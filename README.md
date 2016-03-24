# AngularJS-cheatsheet

### Useful links

* [The Ultimate AngularJS Cheat Sheet - Part 1](http://www.dotnetcurry.com/angularjs/1114/angularjs-cheatsheet-beginner-developers-part1) 
* [The Ultimate AngularJS CheatSheet - Part 2 (Intermediate to Advanced Developers)](http://www.dotnetcurry.com/angularjs/1115/angularjs-cheatsheet-intermediate-advanced-developers-part2) 
* [The Top 10 Mistakes AngularJS Developers Make](https://www.airpair.com/angularjs/posts/top-10-mistakes-angularjs-developers-make) 
* [Opinionated AngularJS styleguide for teams](http://toddmotto.com/opinionated-angular-js-styleguide-for-teams/) 
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

##### To use chrome remote debuggin
1. Open app in device
2. Open chrome
3. type in navegation bar: `chrome://inspect`
4. Select device to inspect
