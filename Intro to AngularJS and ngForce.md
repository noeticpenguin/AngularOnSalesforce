# Introduction to Angular.js and Ngforce 

---

## Seminar 1 - Who's this guy?
## Kevin Poorman, Architect at West Monroe Partners

### @codefriar -- Hey, I just met you, And this is crazy, But here's my twitter, So follow me, maybe!

Hashtags that describe me.
> \#MyBabyDaughterTessa!, \#Homebrew(beer), \#Ruby, \#AngularJS, \#Iot, \#Salesforce, \#!Java, \#!Eclipse, \#FriendsDontLetFriendsUseEclipse 
\#!boringCorporateness

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/KevinP.jpg)

---

## Seminar 1 - Introductions

Lets hear your:
1. Name
2. Buzzword free job role
3. Favorite Food
4. What you hope to learn
5. Your current comfort level with Javascript and Angular

![](http://i.huffpost.com/gen/799955/thumbs/o-THE-MATRIX-AND-HINDUISM-facebook.jpg)

---

## Seminar 1 - Agenda

## Objective - Overview of the Angular.js framework and single page Applications.

## Goals:
1. Overview of Angular.js
1. Loading and Referencing Angular.js within Visualforce.
1. Understanding the ng-app and ng-model directives
1. Basic data binding
1. Angular Expressions

![](http://i1.ytimg.com/vi/iZd6UImTP0g/maxresdefault.jpg)

---

# Seminar 1 - Module 1
## Angular.js Overview.

![](http://i1.ytimg.com/vi/iZd6UImTP0g/maxresdefault.jpg)

---

# Seminar 1 - Module 1

Angular.js is a MVW framwework from Google.
Model-View-*Whatever*
- Model-View :: View-Model
- Model-View-Controller
- Model-View-Whatever

>"the Model of the View"

^ I'm sure everyone's familiar with the standard Model View Controller, three tier framework for developing applications. In your traditional model, you have models classes for data structures and persistence, View classes defining the user interface and (oftentimes, lightweight logic like filtering) and finally, Controller Classes that tie models and views together with buisness logic for fun and profit. 

^ Angular, on the other hand, is a bit more flexible. You can use the standard MVC pattern (and we will) but it's architecture is really more of a MVW, or what I like to call "Model, View, Whatever." In Angular, you'll have definitions for something called Controllers, as well as Models and Markup files designated as Views. However, Controllers are much closer to ViewModels as described by Microsofts' .Net and XAML languages. In this pattern, we have the Model, the View and "the Model of the View"

![](http://i1.ytimg.com/vi/iZd6UImTP0g/maxresdefault.jpg)

---

# Seminar 1 - Module 1
## $scope and state. 

_*$scope*_ Controllers exist soley to define $scope

![inline](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/Presentation1.jpg)

^ Defining $scope is the sole purpose of "controllers" in Angular. All of the $scope variables are available to the view via data bindings. 

^ Views are traditionally .html5 markup files. In our case, we'll be using Visualforce Page files. 

^ Models are perhaps the most complex of the lot, so we'll spend some more time working with them next.

![](http://i1.ytimg.com/vi/iZd6UImTP0g/maxresdefault.jpg)

---

# Seminar 1 - Module 1
## Services, Factories and Providers (or Models).

#Services

```javascript
//Syntax: 
module.service( 'serviceName', function );
```

_Result:_ When declaring serviceName as an injectable argument you will be provided with an instance of the function. In other words

```javascript
new FunctionYouPassedToService();
```

^ Functionally returns a new instance of the service you have specified.
^ In practicality, raw services are rarely used.

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---
# Seminar 1 - Module 1
## Services, Factories and Providers (or Models).

#Factories

```javascript
//Syntax: 
module.factory( 'factoryName', function );
```

_Result:_ When declaring factoryName as an injectable argument you will be provided with the value that is returned by invoking the function reference passed to module.factory.

^ Functionally returns a service, having "constructed" it, as you might expect a Factory method to do.
^ Used Everywhere!

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Services, Factories and Providers (or Models).

#Providers

```javascript
//Syntax: 
module.provider( 'providerName', function );
```

_Result:_ When declaring providerName as an injectable argument you will be provided with ProviderFunction().$get() 

^ Like a factory, the provider service type returns an instance of the service, however this service type can be pre-configured in the applications Runtime file. 
^ Especially useful for broadly scopped services like $http, or Restangular where you'll likely want to preconfigure your service to default to using your own API url.
^ The constructor function is instantiated before the $get method is called - ProviderFunction is the function reference passed to module.provider.

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Services, Factories and Providers (or Models).

General Notes on Factories, Services and Providers:
1. All three can be interchangably referred to as *services*.
2. All are *singletons*. 

^ While Services are different than factories and factories are different than providers, if you look under the angular covers you'll find that all three are created by the same function in the Angular library. As such, they're all considered Services. 

^ Equally important: all services, regardless of their definition are singletons. There will only ever be one instance of AwesomeService, one instance of $http and one of $injector

![](http://i1.ytimg.com/vi/iZd6UImTP0g/maxresdefault.jpg)

---

# Seminar 1 - Module 1
## Services, Factories and Providers (or Models).

When should I use [Service|Factory|Provider]
Use a *Service*, if you want to pass around a given value or object.
Use a *Factory*, if you want to calculate the value or object returned.
Use a *Provider*, if you want to *configure* the service prior to injection. 

^ As mentioned before, by far the most commonly used are factories, as they give the highest flexibiliity for lowest complexity. 

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Services, Factories and Providers (or Models).

What do I use Services for?
1. Sharing code. 
2. Managing state.

> There are only two hard things in Computer Science: cache invalidation and naming things. -- Martin Fowler

^ Two specific uses cases to keep in mind: the sharing of code between services and controllers and managinst state. What do I mean by that?
When managaging state, you will likely have to manage cache. Because they're singletons, cache services can be greatly simplified, and shared across the application.

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Modules and Dependency Inejction

#wth is a module anyway?
- Not a CommonJS module (RequireJS)
- Not a AMD module (Browserify)
- More a container for code
- Basis of the $Injector / Dependency Injection system.

^ Like DI Frameworks in other languages like Spring, or Guice, the DI system in Angular allows you to specify services to be made available to the current module. 

^ Modules have "flavors" and these are what we refrer to as controllers, services and even applications.

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Modules and Dependency Inejction

angular.module()

```javascript
var app = angular.module('demoApp', 
    ['ui.bootstrap', 'ui', 'ui.router',
    'localytics.directives','ngAnimate', 
    'ngForce', 'jmdobry.angular-cache', 'ui.slider',
    'gantt', 'ngSanitize', 'pasvaz.bindonce',
    'xeditable', 'kendo.directives', 'angular-growl'
    ]
);
```

^ This is the first module definition of an Angular applicaiton of any size. This code not only defines the Application module, it dependency injects 15 modules required for the application to work.

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Directives

Directives extend HTML5 in ways that HTML never intended.
Directives each have their own Scope, Template and Controller(s).

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Directives

Directives are:
- The single most complex thing in Angular
- The single most powerful thing in Angular
- Double edge sword

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Directives

Directives are invoked in one of four ways:
1. HTML5 attributes on existing tags 
2. New HTML Elements 
3. Arguments for Selected HTML5 attributes (almost always class="".)
3. Comments

^ By far, directives are most commonly invoked via attributes on existing HTML5 tags, or as their own new html5 elements

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Directives

```html
<li ng-repeat="c in contacts"> </li>
<awesomeSauceDirective/>
<!-- ng-repeat="c in contacts" -->
```

More on Directives later. 

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Directives

## Three Key Directives that you *must know*
1. ng-app
2. ng-controller
3. ng-model

^ there are 3 directives that you must know - without them it's not possible to run an Angular application. 
The first, ng-app is what tells Angular to step in and start processing directives and bindings. Remember our Application module, how we specified its name as demoApp, this is where we specify the demoApp module for instantiation. This is the bootstrap of the application.
^ ng-Controller binds a controller to a given html tag and it's children. 
^ ng-Model binds an element, say an text input box, to a coresponding $scope variable.

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Directives

```html
<body ng-app="demoApp">
  <div ng-controller="demoController">
    <form name="testForm" ng-controller="ExampleController">
      <input ng-model="val" />
    </form>
  </div>
</body>
```

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Binding

![right 100%](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training//Two_Way_Data_Binding.png)

- Angular provides seemless, automatic binding between scope and views. 
- This binding is two-way.
- Done (currently via Dirty Checking.

^ Two way binding means that if the value of the view changes, so does the value of the controller, and vice versa. 
^ Dirty Checking means that Angular has a digest cycle where by every so many ticks, it checks to see if the controller or the view has changed the value amd mirrors them to other. 
^ As you might imagine, performance degrades with the # of dirty checks needed. Keeping the dirty checking to a minimum ensures application performance. You're unlikely to hit performance issues with dirty checking, however, until you try to do something like display a spreadsheet like grid of 22x50 data cells. (1,100 dirty checks per tick)


---

# Seminar 1 - Module 1
## Binding

```html
<ul class="phones">
  <li ng-repeat="phone in phones">
    <span>{{phone.name}}</span>
    <p>{{phone.snippet}}</p>
  </li>
</ul>
```

^ Here we have an unordered list definition invoking the ng-repeat directive and binding using the standard {{}} double mustache binder.
^ There are other ways to bind, including directives for bindingOnce, or rather binding only on first evaluation.

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

# Seminar 1 - Module 1
## Filtering 

```html
<ul class="phones">
  <li data-ng-repeat="phone in phones">
    <span>{{phone.name}}</span>
    <p>{{phone.snippet}}</p>
  </li>
</ul>
```

^ Here we have an unordered list definition invoking the ng-repeat directive and binding using the standard {{}} double mustache binder.
^ There are other ways to bind, including directives for bindingOnce, or rather binding only on first evaluation.

![](file:///Users/kpoorman/copy/Presentations/Angular and NgForce Training/sublimeTextCode.jpg)

---

#Seminar 1 - Module 1
## Loading Angular

- Use the HTML5 doctype
- Turn off sidebar and header
- Static resource bundles v. CDN v. inline

^ While Angular itself doesn't conflict with the JS inserted into visualforce pages by Salesforce, many of the visual component libraries do. For our work this week, want to always tell visualforce to disable the sidebar and header, and we want it to use the html5 doctype. 
^ Additionally, we want to utilize static resource bundles within Salesforce to store our JS, and CSS code. 

---

#Seminar 1 - Module 1
## Loading Angular

```html
<apex:includeScript value="
    <!-- ng is the name of the Static resource -->
    {!URLFOR($Resource.ng, 
    'folder/file.js')}"/>
<!-- For Example: -->
<apex:includeScript 
    value="{!URLFOR($Resource.ng, 
        'pp_js/angular.min.js')}"/>
```

^ We're going to put our loading code in a custom Visualforce Component. We'll be able to inject that component on any page that we need to use Angular on.

---

#Seminar 1 - Lab 1
## Build an Angular hello world app on Visualforce that:
- Has an Application module
- Has a controller
- Has a single view
- Loads Angular from a Vf Component that pulls from a Static Resource bundle
- Has an input field, bound to a $scope variable

---

#Seminar 1 - Lab 1
## Build an Angular hello world app on Visualforce that:
- Dependency Inject the bootstrap and chosen modules into your app
- Render a "chosen" based directive bound to an array of us States.

---
#Resources
## Things needed for Lab 1

https://github.com/localytics/angular-chosen
http://angular-ui.github.io/bootstrap/

---

#Seminar 1 - Seminar 2
## Advanced Angular and Built-ins

## Goals:
1. Stay classy Cleaveland.
2. Filters.
3. Advanced views and partials.
4. UI Router
5. Built-in Directives
6. Watch me

---

#Seminar 1 - Seminar 2
## Advanced Angular and Built-ins

## Staying Classy - A better way of defining controllers
Traditional controller module

```javascript
app.controller('AppCtrl',
  ['$scope', '$location', '$http',
  function($scope, $location, $http) {
    // ...
}]);
```

^ Quick recap, here we define a module of type controller, with the name AppCtrl, injecting $scope, $location and $http services. Not the clearest syntax, but def. servicable. 

---

#Seminar 1 - Seminar 2
## Advanced Angular and Built-ins

## Staying Classy - A better way of defining controllers
Classy Controller Module

```javascript
app.classy.controller({
  name: 'AppCtrl',
  inject: ['$scope', '$location', '$http'],
  //...
});
```

^ Compare our traditional controller module to this one, utilizing Classy, an add on to Angular that I highly recommend. 
Here we self-document the Name, and the services we're injecting directly. Makes bringing new team members upto speed much quicker, as it's much more explicit. 

---

#Seminar 1 - Seminar 2
## Staying Classy - A better way of defining controllers
- functions not prepended with _ are automatically added to scope
- functions starting with _ are considered private and not on the scope
- this represents the controller, and this.$ is shorthand for scope
- Enables Reverse Reference controllers

^ Classy also adds all methods who's name doesn't start with an underscore to the scope, likewise those that do start with an underscore are considered private.
^ the keyword this refers to the controller, as you might expect
^ this.$ is a shorthand for scope.
^ referse reference controllers enable you to bind controllers to element by css selector, rather than by invoking the ng-controller directive.

---

#Seminar 1 - Seminar 2
## Filters and filter chains

- Builtin and custom
- Uses the unix "pipe" theory and syntax

```html
<table id="searchObjResults">
  <tr><th>Name</th><th>Phone</th></tr>
  <tr ng-repeat="friendObj in friends | filter:search:strict">
    <td>{{friendObj.name}}</td>
    <td>{{friendObj.phone}}</td>
  </tr>
</table>
```

---

#Seminar 1 - Seminar 2
## Filters and filter chains

Common Builtin Filters:
currency, date, json, limitTo, lowercase / uppercase, number, orderBy

Chainable:

```javascript
<tr ng-repeat="friendObj in friends | currency | orderBy:value">
```

---

#Seminar 1 - Seminar 2
## Advanced Views with Visualforce

- Partials can mix and match HTML5 and Visualforce
- Attach standard / custom controllers and extensions
- Use apex components and Visualforce merge fields
- Poor-man's data bridge 

^ When running Angular on Salesforce, you can use Visualforce pages as view templates and partials. It's key, however, that you use HTML5 doctype and disable the header and sidebar. Likewise, you can mix in Visualforce components, which we'll use to load our javascript. 
^ Lastly, You should know that you can always broadcast from Apex/Visualforce to Angular by serializing objects and variables to JSON and merging those variables into the pages. This is a useful technique for snagging the REST api's OAuth key. 

---

#Seminar 1 - Seminar 2
## Advanced Views with Visualforce

```javascript
<apex:page 
showHeader="false" 
sidebar="false" 
standardStylesheets="false" 
docType="html-5.0" 
applyHtmlTag="false"> 
{!mergeField} 
<!-- angular.fromJson({!jsonMergeField}); -->
</apex:page>
```

---

#Seminar 1 - Seminar 2
## Single Page Applications and UIRouter

UIRouter is an alternative routing package from the Angular UI team. 
Works on application state rather than given views.

^ Single Page Applications or SPAs built with tools like Angular don't normally navigate between pages, but between views. 
^ AngularUI Router is a routing framework for AngularJS, which allows you to organize the parts of your interface into a state machine. Unlike the $route service in the Angular ngRoute module, which is organized around URL routes, UI-Router is organized around states, which may optionally have routes, as well as other behavior, attached.
^ in practice these are remarkably alike, however, state based routing is far more powerful, offering nested views and complex state transforms.

---

#Seminar 1 - Seminar 2
## Single Page Applications and UIRouter

```javascript
var myApp = angular.module('myApp', ['ui.router']);
// Like all our components, and even the build in $route service
// You need to DI the ui.router
```

---

#Seminar 1 - Seminar 2
## Single Page Applications and UIRouter

```javascript
$stateProvider
   .state('state1', {
     url: "/state1",
     templateUrl: "partials/state1.html"
   })
   .state('state1.list', {
     url: "/list",
     templateUrl: "partials/state1.list.html",
     controller: function($scope) {
       $scope.items = ["A", "List", "Of", "Items"];
     }
   })
   .state('state2.list', {
     url: "/list",
       templateUrl: "partials/state2.list.html",
       controller: 'State2Ctrl'
     })
   });
   $urlRouterProvider.otherwise("/state1");
```

^ With UIRouter, we define states as the code here shows. Each state defines not only it's name, but also an object defining it's url, template url and controller. 
^ note that you can give each state it's own controller either by passing in a controller name, or by defining it inline. (inline is not preferred.)

---

#Seminar 1 - Seminar 2
## Single Page Applications and UIRouter

```html
<!-- index.html -->
<body>
    <div ui-view></div>
    <!-- We'll also add some navigation: -->
    <a ui-sref="state1">State 1</a>
    <a ui-sref="state2">State 2</a>
</body>
```

^ On the view side of things, we have to tasks to enable UIRouter.
First, define one or more divs with the ui-view directive enabled.
Secondly, provide links, buttons or other ui Elements tagged with the ui-sref directive. 

---

#Seminar 1 - Seminar 2
## Angular.js Builtin _*attribute*_ Directives (freebies)

-ng-href
-ng-src
-ng-disabled
-ng-checked
-ng-readonly
-ng-selected
-ng-class
-ng-style

^ all of these match their standard HTML5 look alikes, but are hooked into the Angular digest cycle. Because of this they can utilize Controller variables and expressions
^ ng-class / ng-style for instance allow you to conditionally add css styles or classes to elements.
^ ng-disabled, and ng-readonly conditionally set form elements as read only or disabled based on the directives given expression.

---

#Seminar 1 - Seminar 2
## Angular.js Builtin _*attribute*_ Directives (freebies)

```html
<input type="text" ng-model="someProperty" placeholder="Type to Enable">
<button ng-model="button" ng-disabled="!someProperty">A Button</button>
<!-- Always use ng-href when href includes an {{ expression }} -->
<a ng-href="{{myHref}}">I'm feeling lucky, when I load</a>
```

---

#Seminar 1 - Seminar 2
## Angular.js Builtin Directives (freebies)

Other directives of note:

- ng-if
- ng-cloak
- ng-hide / ng-show
- ng-bind (== {{}})

^ You'll end up using these directives all the time, so take note of them. 
ng-if not conditionally adds or removes dom elements based on the truthiness of the expression. If, for instance you have a div that you'd like to hide until a user presses a button, ng-if can be used to *entirely* remove the div from the DOM until the condition is met. 
^ ng-Cloak also works with view visability - pausing the display of the div until Angular has completely finished processing the bind expressions. This is helpful for preventing flickering where the template appears, and then the data backing the template appears.
^ ng-Hide and ng-Show are actually the same directive, invoked inversely to one another. If you've an element that is defaulting to hidden, and you want to display it when a given condition is met, ng-show is the directive you want to use. If the inverse is true, and you want to hide a visible element, ng-hide will allow you to do that.
^ When writing your own custom directives, it can often be clearer to use the ng-bind directive than the {{}}. Additionaly, in highly dynamic (reflective / metaprogrammed) situationes I Highly highly suggest you use ng-bind.

--- 

#Seminar 1 - Seminar 2
## Angular.js Builtin Directives (freebies)

Other directives of note:

```html
<span ng-if="checked" class="animate-if">
  I'm removed when the checkbox is unchecked.
</span>
<div id="template1" ng-cloak>{{ 'hello' }}</div>
<div ng-show="myValue"></div>
<div ng-hide="myValue" class="ng-hide"></div>
Hello <span ng-bind="name"></span>!
```

--- 

#Seminar 1 - Seminar 2
## Watch Expressions

Watch expressions allow you execute code reactively. 
Think about them as triggers for Angular

```javascript
var scope = $rootScope;
$scope.name = 'misko';
$scope.counter = 0;
scope.$watch('name', function(newValue, oldValue) {
  scope.counter = scope.counter + 1;
});
```

^ Here we're watching the scope variable 'name' and calling an anonymous function to increment the scope variable counter, everytime $digest runs. The $digest cycle restarts evertime it detects a change, so if you have multiple edits happening in succession the watch expression function may fire multiple times as well.
^ watch expressions add to your dirty checking time as well.

--- 

#Seminar 1 - Seminar 2
## Watch Expressions

Classy makes watch expressions easier too.

```javascript
watch: {
  'location.path()': function(newValue, oldValue) {
    // ...
  },
  '{object}todos': function (newValue, oldValue) {
    // ...
  }
}
```

^ Note the {object} tag on the second watch expression. You can tell classy to utilize different types of watch expressions -- for objects or collections.

--- 

#Seminar 1 - Lab 2
## Classy controllers, Multiple states, watch expressions and built in directives

- Refactor your application module to use ui-router.
- Define at least two states, home and detail.
- Setup classy controllers for your states.
- Create visualforce pages for your partials


--- 
#Seminar 1 - Lab 2
## Classy controllers, Multiple states, watch expressions and built in directives

- Embed your partials and router inside a master app visualforce page.
- Use a watch expression to count the number of times you navigate between states
- Use ng-if and ng-hide or show to conditionally display a div when you click on a button 

