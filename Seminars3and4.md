# AngularJS Training
## https://github.com/noeticpenguin/AngularOnSalesforce

---

# Seminar 3 - Module 1
## Agenda & Goals

- Building Factories, Services and Providers
- Building Directives
- Building Filters
- Promises promises

---

# Seminar 3 - Module 1
## Building Factories, Services and Providers

- Services in general are modules specifically tied to a given application module

```javascript
app.factory('nameService', function(dependencies, go, here) {
    var nameService = {
        Properites: here,
        Method: function(){},
    }
    return nameService;
});
```

^ Note that we create a variable "nameService" then return it. The nameService object can be configured, initialized or setup however needed so long as the object is returned in the state you need.

---

# Seminar 3 - Module 1
## Building Factories, Services and Providers


```javascript
app.provider('remoteObjects', ['$q','$log',
  function ($q, $log) {
    this.$get = function () {
        //-------------------
        // default namespace
        var namespace = "SObjectModel";
        // app.config setter for the namespace.
        this.setNamespace = function (newNamespace) {
          if(!_.isUndefined(newNamespace)) {
            namespace = newNamespace;
          }
        };
        //--------------------
        var remoteObjects = {}
        return remoteObjects;
    };
}]);
```

^ Note the odd this.$get method? It returns an object just like the Factory, but this one includes other variables outside of the returned object. These variables, and their setter methods allow you to manipulate the configuration of the service in the one-time run app.config method. 

---

# Seminar 3 - Module 1
## Directives

- Directives combine a View + Directive defintion as well as optional Directional controllers to extend HTML.
- To future proof your directives against future HTML attributes and tags, prefix your directives. 
- Directives definition modules return a factory function. 

^ Directives are compiled, and by that we mean attaching watch statements, and event listeners to the Directives template. These make the directive interactive.

---

# Seminar 3 - Module 1
## Basic Directive Defintion

```javascript
app.directive('userInfo', function() {
  return {
    restrict: 'E', // means this must be it's own element also see [A|C|AEC]
    templateUrl: '/apex/userInfoTemplate'
  };
});
```

^ The anonymous factory function defined on line 1 is referred to as the Compile function as it's called when the directive is compiled, or applied to the element.

^ the scope: {} object 

---

# Seminar 3 - Module 1
## Basic Directive Defintion

```html
Name: {{userInfo.name}} email: {{userInfo.email}}
```

^ The anonymous factory function defined on line 1 is referred to as the Compile function as it's called when the directive is compiled, or applied to the element.

---

# Seminar 3 - Module 1
## Basic Directive Defintion

- Unfortunately, this directive will share the same scope amongst all instances of the directive.
- Every time the <userInfo/> directive is invoked, the data passed into it is shared. 
- Ng-repeat over 5 Users will result in the directive showing 5 instances of the last user.

---

# Seminar 3 - Module 1
## Basic Directive Defintion

To solve this, use an isolated scope.

```javascript
app.directive('userInfo', function() {
  return {
    restrict: 'E', // means this must be it's own element also see [A|C|AEC]
    scope: {userInfo: '=info'}, // LocalScopeVar: '=attributeName'
    templateUrl: '/apex/userInfoTemplate'
  };
});
```

^ You can also bind the directive level scope variable to an attribute of the same name by using a plain '=' sign.
^ best practice: always use isolated scope.

---

# Seminar 3 - Module 1
## Basic Directive Defintion

To solve this, use an isolated scope.

```javascript
app.directive('userInfo', function() {
  return {
    restrict: 'E', // means this must be it's own element also see [A|C|AEC]
    scope: {userInfo: '&info',
            someFunc: '&ServiceName.Functioncall'},
    templateUrl: '/apex/userInfoTemplate'
  };
});
```

^ While we use = to isolate the scope for a given variable, we can use the andpersand to bind directive scope.
^ Any valid ng expression is allowed, and as you can see we can even bind to a function in a service. 
^ this is useful for sharing code between services and directives as well as allowing you to swap in functionality depending on scope. Imagine a directive that has a call back function that you can specify on the fly. 

---

# Seminar 3 - Module 1
## Basic Directive Defintion

Transclude: It's not just for breakfast.

```javascript
app.directive('userInfo', function() {
  return {
    restrict: 'E', // means this must be it's own element also see [A|C|AEC]
    scope: {}, // LocalScopeVar: '=attributeName',
    transclude: true,
    templateUrl: '/apex/userInfoTemplate',
    link: function (scope, element) {
      $scope.user = {name: "Kevin", email: "kjp@codefriar.com"}
    }
  };
});
```

^ Transclude inverts the isolated scope statement, giving the directive access to the scope invoking the directive.
^ link function accepts the scope defined by the directive, and the element the directive is fired upon and act as a lightweight controller.
^ Notice the link function's definition of $scope.user? Logically, we'd assume then that the directives understanding of $scope.user would come from the link function but when transclude is turned on, the definition of user will actually be defined by the controller in which the directive is placed.
Best practice: use & to expose an "api" of your directive.

---

# Seminar 3 - Module 1
## Basic Directive Defintion

Slightly obscure Directive options

```javascript
app.directive('userInfo', function() {
  return {
    restrict: 'E', // means this must be it's own element also see [A|C|AEC]
    scope: {userInfo: '@'}, // ie: userInfo="{{user}}"
    require: ^fooBarBaz,
    templateUrl: '/apex/userInfoTemplate',
    link: function (scope, element) {
    }
  };
});
```

^ Two quick things to point out here. First is the scope object specified as an @ sign. @ binds the scope value to the evaluated expression of it's directive attribute. If you want to be able to interpolate scope variables into your attributes, use @
Secondly, Notice the new require clause. When you use require, you can specify that the directive has, or is a child of an element with a given controller. the carrot, means "this directive is a child of an element with the Named controller" If you have a require line without the carrot, it means this directive must implement the given controller.  

---

# Seminar 3 - Module 1
## Basic Directive Defintion

Directive Controllers

```javascript
app.directive('myTabs', function() {
    return {
      restrict: 'E',
      transclude: true,
      scope: {},
      controller: function($scope) {
        var panes = $scope.panes = [];
        $scope.select = function(pane) {};
        this.addPane = function(pane) {};
      }
    };
  })
```

^ Until now all of our directive examples have used a link: function(), but thats not the only option we have. We can replace our link function with a controller function, if we want to expose methods and properties from our directive to other directives. This example comes from a pair of directives that, when used together handle the creation of tabs.
^ when you have nested directives like this, the link function of the inner most directive is automatically passed a reference to the controller of the enclosing directive. 

---

# Seminar 3 - Module 1
## Basic Directive Defintion

Directive Controllers

```javascript
app.directive('myPane', function() {
    return {
      require: '^myTabs',
      restrict: 'E',
      transclude: true,
      scope: {
        title: '@'
      },
      link: function(scope, element, attrs, tabsCtrl) {
        tabsCtrl.addPane(scope);
      },
      templateUrl: 'my-pane.html'
    };
  });
```

^ when you have nested directives like this, the link function of the inner most directive is automatically passed a reference to the controller of the enclosing directive. 

---

# Seminar 3 - Module 1
## Basic Directive Defintion

Directive Recap

- Directives Extend HTML5 elements and attributes
- Can be invoked as elements, attributes, classes and comments
- Can, and should, have isolated scope.
- Can be bound to a controller or a link function
- Transclude inverts the scope definition

---

# Seminar 3 - Module 1
## Filters 

```javascript
app.filter('Autolink', ['$sce', function($sce){
    var urlPattern = /(http|ftp|https):\/\/[\w-]+(\.[\w-]+)+([\w.,@?^=%&amp;:\/~+#-]*[\w@?^=%&amp;\/~+#-])?/gi;
    return function(text, target, otherProp) {
        if(text === undefined ||
                text === null ) { return $sce.trustAsHtml(text);}
        angular.forEach(text.toString().match(urlPattern), function(url) {
            text = text.replace(url, "<a target=\"" + target + "\" href="+ url + ">" + url.substring(0,30) +"</a>");
        });
        return $sce.trustAsHtml(text);
    };
}]);
```

^ Filters are defined exactly like a directive or a controller, as a function with DI'd dependencies. 
^ Filters must have a return function, that returns a value. 
^ This filter takes a given block of text and automatically generates html links for http links in the text. 
^ Filters are often thought of as fuctions that return a subset of data from a collection, but in reality they are transient data mutators. They're under-used and way more powerful than most people realize. 
Best practice: Always determine if the directive you're wanting to write can be written as a filter.

---

# Seminar 3 - Module 1
## Promises 

The Problem: 

- Computer programming languages are synchronous. 
- Instructions that take multiple seconds to complete cause the entire application to pause until it's completed.


---

# Seminar 3 - Module 1
## Promises 

The Solution:

- Javascript, in general, is *not* synchronous, but asynchronous for api requests. (XMLHttpRequests)
- The good: Program Execution does not pause while api calls complete. 
- The bad: Program Execution does not pause while api calls complete.


---

# Seminar 3 - Module 1
## Promises 

The ugly: 
> "condition is Fact: 9 out of 10 developers don't know what a race" - @iamdeveloper

---

# Seminar 3 - Module 1
## Promises 

Traditionally we solve this new problem with callbacks:

```javascript
MakeApiCall(apiCall, function(input1, input2){
    MakeApiCall(apiCall2, function(data){
            MakeApiCall(apiCall3, function(data){
                ... 
            });
        });
    });
```

---

# Seminar 3 - Module 1
## Promises 

Promises decouple the long-running call, with the callback function. 

```javascript
var pQuery = vfr.query("SELECT id, Name FROM Contact");
pQuery.then(function(data){
    //stuff.
});
```

^ prevents the march to the right, and enables cleaner code thats easier to read.
^ The concept is for promises is taken from language. the Object promises to X, and let you know when it's done. 

---

# Seminar 3 - Module 1
## Promises 

Additional features of Promises 

- Chaining. Do X, then Y, then Z, then AA, then AB...
- Error handling. Do X, then Y ... Error Handler

^ I draw attention to the chaining aspect because the error handling aspect works by chaining. 

---

# Seminar 3 - Module 1
## Promises 

```javascript
var pQuery = vfr.query("SELECT id, Name FROM Account");
pQuery.then(function(data){
    return pQuery2 = vfr.query(
        "SELECT Id, Name FROM Contact WHERE AccountID in ('" + 
        _.pluck(data, "Id").join("', '") + "'");
}).then(function(data2){
    // ...
}, function(error){
    $log.log(error);
});
```

---

# Seminar 3 - Module 1
## Promises 

Done properly Chaining allows your code to look like this:

```javascript
promise
  .then(doSomething)
  .then(doSomethingElse)
  .then(doSomethingMore)
  .catch(logError);
```

^ best practice: (or at least a strong convention) is to indent and start your line with the period when using fluid interfaces like this. 

---

# Seminar 3 - Module 1
## Promises 

Lets see that in a real use case

```javascript
CustomerService.getCustomer(currentCustomer)
    .then(CartService.getCart) // getCart() needs a customer object, returns a cart
    .then(calculateTotals)
    .then(CheckoutService.createCheckout) // createCheckout() needs a cart object, returns a checkout object
    .then(function(checkout) {
      $scope.checkout = checkout;
    })
    .catch($log.error)
```

---

# Seminar 3 - Module 1
## Promises 

```javascript
var deferred = $q.defer();
var promise = deferred.promise;
 
// resolve it after a second
$timeout(function() {
  deferred.resolve('foo');
}, 1000);
 
promise
  .then(function(one) {
    console.log('Promise one resolved with ', one);
 
    var anotherDeferred = $q.defer();
 
    // resolve after another second
 
    $timeout(function() {
      anotherDeferred.resolve('bar');
    }, 1000);
 
    return anotherDeferred.promise;
  })
  .then(function(two) {
    console.log('Promise two resolved with ', two);
  });
```

^ note the $q.defer() call, this generates an empty *defered* object
^ promises are generated from the deferred.promise call.
^ the various then blocks return their own promises, allowing the chain to continue. 
^ promise flow control.

---

# Seminar 3 - Module 1
## Additional Promise Features

```javascript
$q.all([promiseOne, promiseTwo, promiseThree])
  .then(function(results) {
    console.log(results[0], results[1], results[2]);
  });
```

^ All this work needs to be done before I can do XYZ, but I don't care what order they run in.

---

# Seminar 3 - Module 1
## Additional Promise Features

```javascript
$q.when('foo')
  .then(function(bar) {
    console.log(bar);
  });
 
$q.when(aPromise)
  .then(function(baz) {
    console.log(baz);
  });
 
$q.when(valueOrPromise)
  .then(function(boz) {
    // well you get the idea.
  })
```

^ use $q.when to wrap arbitrary values in a promise, or when you're not sure if the value you've got has resolved.

---

# Seminar 3 - Module 1
## NgForce Overview

- VFR
- SFR / SFRQuery
- RemoteObjects

^ We're going to walk through the code on this one.

---

# Seminar 3 - Module 1
## Heads up on Debugging.

- Using Chrome or Safari? Install the ng-inspector plugin!
- Get a handle on an Angular Scope for an element via:
    + angular.element('#name').scope();
    + angular.element('.class').isolateScope();
- Grab ANY Service with:
    + angular.element('html').injector().get('MyService')

^ We talked about the first one yesterday, lets demo its use in a full blown app.
^ rest are done in the JS console. 

---

# Seminar 3 - Module 1
## Heads up on Debugging.

- Access the Controller of a directive with:
    + angular.element('my-pages').controller()
- Chrome Only: $0 - $4 == Last 5 selectors

---

# Seminar 3 - Lab 1
## Lab 

We're going to:
- Build a Factories service that models out Contacts.
- Build a Directive that displays our Contacts.
- Build a filter to build links out of email addresses.
- Throughout we'll be utilizing promises.


