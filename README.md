angular-component-factory [![Build Status](https://travis-ci.org/kennethlynne/angular-component-factory.png?branch=master)](https://travis-ci.org/kennethlynne/angular-component-factory)
=========================
[![Stories in Ready](https://badge.waffle.io/kennethlynne/angular-component-factory.png?label=ready)](http://waffle.io/kennethlynne/angular-component-factory)

A VERY opinionated AngularJS wrapper to simplify use of directives as components.

See [demo](http://kennethlynne.github.io/angular-component-factory/), and view source.

## Usage
To install: ```bower install angular-component-factory```

Then add ```componentFactory``` as a dependancy to your module or decorate module using ```angular.componentFactory.moduleDecorator(<your module>)```

###Example:
```
var demo = angular.module('DemoApp', []);

angular.componentFactory.moduleDecorator(demo);

demo.component('unicornTest', function ($log) {
    $log.log('Demo component says hi!')

    return {
        link: function (scope) {
            scope.message = "I'm a unicorn component!";
        }
    }
});
```

views/components/unicorn-test/unicorn-test.html:
```
<p>{{message}}</p>
```

```
<unicorn-test-component></unicorn-test-component>
```

Will now expand into a directive that by convention loads a template from ```/views/components/unicorn-test/unicorn-test.html```, replaces contents by default, and creates a new scope.
The object that the constructor returns should be a directive definition object. This will then be decorated for convention over configuration.

One way you might use it:

 ```
 demo.component('horsePartial');
 ```

This will try to fetch a template from ```/views/components/horse-partial/horse-partial.html``` and instantiate a directive with a isolate scope.
Later on when you want to add some logic, just implement the constructor function as you usually do for a directive.

## Configuration
If you want to set the view path you may do something like this
```
demo.config(function (componentFactoryProvider) {
    componentFactoryProvider.setViewPath('some/custom/path/');
});
```
Views will then be loaded from
```
some/custom/path/unicorn-test/unicorn-test.html
```

You can also provide `setViewPath` with a factory function to do more custom handling based on the components name:

```
componentFactoryProvider.setViewPath(function (componentSnakeCasedName, componentName) {
    return 'components/' + componentSnakeCasedName + '/some-path/views/' + componentName + '.html';
});
```
This function will be called on every registration to map the component name to a url.
The above example will return 'components/example-name/some-path/views/exampleName.html'

The factory will be served with two parameters: the snake cased component name, and the original one, and is expected to return a string path.