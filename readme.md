Klass [![NPM version](https://badge.fury.io/js/klass.png)](http://badge.fury.io/js/klass)
=======
This module provides some syntax sugar for "class" declarations, constructors, mixins, "super" calls and static members.

Getting Started
---------------
###In Node.js###
You can install using Node Package Manager (npm):

    npm install klass

###In Browsers###
```html
<script type="text/javascript" src="klass.js"></script>
```
It also supports RequireJS module format and [YM module](https://github.com/ymaps/modules) format.

Module has been tested in IE6+, Mozilla Firefox 3+, Chrome 5+, Safari 5+, Opera 10+.

Specification
-------------
###Creating a base class###
````js
Function klass(Object props);
````
###Creating a base class with static properties###
````js
Function klass(
    Object props,
    Object staticProps);
````
###Creating a derived class###
````js
Function klass(
    Function BaseClass,
    Object props,
    Object staticProps);
````
###Creating a derived class with mixins###
````js
Function klass(
    [
        Function BaseClass,
        Function Mixin,
        Function AnotherMixin,
        ...
    ],
    Object props,
    Object staticProps);
````

Example
------------
```javascript
var klass = require('klass');

// base "class"
var A = klass(/** @lends A.prototype */{
    __constructor : function(property) { // constructor
        this.property = property;
    },

    getProperty : function() {
        return this.property + ' of instanceA';
    },
    
    getType : function() {
        return 'A';
    },

    getStaticProperty : function() {
        return this.__self.staticProperty; // access to static
    }
}, /** @lends A */ {    
    staticProperty : 'staticA',
    
    staticMethod : function() {
        return this.staticProperty;
    }
});

// klassed "class" from A
var B = klass(A, /** @lends B.prototype */{
    getProperty : function() { // overriding
        return this.property + ' of instanceB';
    },
    
    getType : function() { // overriding + "super" call
        return this.__base() + 'B';
    }
}, /** @lends B */ {
    staticMethod : function() { // static overriding + "super" call
        return this.__base() + ' of staticB';
    }
});

// mixin M
var M = klass({
    getMixedProperty : function() {
        return 'mixed property';
    }
});

// klassed "class" from A with mixin M
var C = klass([A, M], {
    getMixedProperty : function() {
        return this.__base() + ' from C';
    }
});

var instanceOfB = new B('property');

instanceOfB.getProperty(); // returns 'property of instanceB'
instanceOfB.getType(); // returns 'AB'
B.staticMethod(); // returns 'staticA of staticB'

var instanceOfC = new C();
instanceOfC.getMixedProperty() // returns "mixed property from C"
```
