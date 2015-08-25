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
