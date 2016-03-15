# $scope.$watch and $scope.$watchCollection

## Overview

We can observe changes to variables using `$scope.$watch` - this is awesome, but we need to make sure we're smart with them!

## Objectives

- Describe watching Models with $scope.$watch
- Describe $scope.$watchCollection
- Describe deep Model watching
- Write a $scope.$watch function
- Write a $scope.$watchCollection function

## Watching models

In our controllers, we can watch when values update using `$scope.$watch`. We pass through two functions to this - the first one is a function that returns the value we want to watch. The second is the function that gets called when the value is updated.

```js
function SomeController($scope) {
	var ctrl = this;
	ctrl.search = '';

	$scope.$watch(function () {
		return ctrl.search;
	}, function (newValue, oldValue) {
		console.log('value updated!');
	});
}

angular
	.module('app')
	.controller('SomeController', SomeController);
```
```html
<input ng-model="ctrl.search" />
```

When a user types into our input, our function will get called with it's old value and it's new value. For instance, if we typed the letter `r` into our input, it would get called with `oldValue` equal to `''` and `newValue` equal to `'r'`.

## Watching objects/arrays

We've looked at observing strings, now let's watch objects/arrays. We can do this with ``$scope.$watchCollection`


```js
function SomeController($scope) {
	var ctrl = this;
	ctrl.collection = {
		id: 5,
		name: 'Test'
	};

	$scope.$watchCollection(function () {
		return ctrl.collection;
	}, function (newValue, oldValue) {
		console.log('value updated!');
	});
}

angular
	.module('app')
	.controller('SomeController', SomeController);
```

This will then fire out whenever we change any item in that object! However, this doesn't work that well when we change a value inside an object inside an object.

## Deep-watching objects/arrays

We can deep watch objects/arrays by going back to `$scope.$watch`, but passing in a third parameter of `true` to tell Angular to deep watch the collection.

```js
function SomeController($scope) {
	var ctrl = this;
	ctrl.collection = {
		id: 5,
		name: 'Test',
		country: {
			name: 'United States'
		}
	};

	$scope.$watch(function () {
		return ctrl.collection;
	}, function (newValue, oldValue) {
		console.log('value updated!');
	}, true);
}

angular
	.module('app')
	.controller('SomeController', SomeController);
```

Before, with `$scope.$watchCollection`, if we had updated the country's name, we wouldn't have got a callback fired. However, with `$scope.$watch`, we do! Just be careful - this requires more resources to check the object, so use them only when you must.
