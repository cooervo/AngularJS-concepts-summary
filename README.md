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
