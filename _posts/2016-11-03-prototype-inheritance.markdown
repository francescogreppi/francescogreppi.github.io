```
layout: post
title:  "Prototypal Inheritance and prototypal chain"
date:   2016-11-02 14:00:00 +0100
categories: Javascript
comments: false
```

In Javascript a `prototype` is an object from which other objects inherits properties. Every object has its own prototype and, since a prototype is an object too, 
every prototype has its own prototype (exception given for the default object prototype at the top of the prototype chain, which is `null`, we'll see this shortly).

The prototype of an object is referenced in the internal property `[[Prototype]]` which can be accessed in three ways: `.constructor.prototype`, `__proto__` or 
via the `getPrototypeOf()` method of the Javascript built-in `Object` constructor. Let's see it in action:

```javascript

    var a = new Object();
    a.name:'Mark';
    a.surname:'Twain';
    // in this example the var a
    // inherits the properties of the js built-in Object constructor
    // in fact a is an instance of the Object constructor
    
    a.__proto__; // Object { ..., constructor: function Object(), ...}
    Object.getPrototypeOf(a); // Object { ..., constructor: function Object(), ...}
    a.constructor.prototype; // Object { ..., constructor: function Object(), ...}

```

Now, let's consider a slightly different example. 

```javascript

    function Person (name, surname){
        this.name = name;
        this.surname = surname;
    }

    var b = new Person ('Mark', 'Twain'); 

    b.__proto__; // Object { ..., constructor: function Person(), ...}
    Object.getPrototypeOf(b); // Object { ..., constructor: function Person(name, surname), ...}
    b.constructor.prototype; // Object { ..., constructor: function Person(name, surname), ...}
    
```

Notice here that the `prototype` is not anymore the Object constructor but our custom constructor `Person`. Now, if we want to check what is the prototype of our custom constructor we simply have to use one of the three conventional way mentioned above on the `Person` constructor. Let's see what happens:

```javascript

    Person.__proto__; // function(){}
    
```

Yes, the prototype of a constructor is a function. In fact, a constructor is a function and as such it inherits the properties of the javascript `Function` object when created. 
This is not only valid for our custom `Person` constructor but also for the built-in `Object` constructor.

```javascript

    Object.__proto__; // function(){}
    
```

So, what is the prototype of the prototype of a constructor? Isn't it like to say the prototype of the `Function` object? 
It is indeed. In fact, if we try the following:

```javascript

    Object.__proto__.__proto__; // object{}
    Person.__proto__.__proto__;; // object{}
    
```

Yes, we get object{}, because 'Function' is the built-in Javascript object used to instantiate a function. What's next?
If we try to see what is the prototype of the primordial object we get `null`, as anticipated at the beginning of the article.

```javascript

    Object.__proto__.__proto__.__proto__; // null
    Person.__proto__.__proto__.__proto__; // null
    
```

We made it, we get to the very beginning of the prototypal chain, after which no other prototype can be retrieved. However, so far we've seen example where we create objects by using constructors. What happens when I create an object literal? 

```javascript

    var a = {
        name: 'Mark',
        surname: 'Twain
    }

    a.__proto__; // Object { ..., constructor: function Object(), ...}

```

Same thing as in the first example. That's because when you declare a literal, behind the scene, Javascript invoke the Object constructor to create our instance.


### Function prototype ###

In all of this there is a part of Javascript which is very confusing. This is the **built-in Function property called prototype**, is it the same thing of the prototype we
talked about so far? Yes and no. As said, in the case of a function the prototype is a built-in property, and it's made to reflect the constructor's properties on the instances created.
You can see this happening when you create instances from a constructor and then modify the constructor, for instance by adding a property.

```javascript

    function Animal(name){
        name = this.name; 
    }
    var dog = new Animal ('dog');
    Animal.prototype.action = 'bark';
    dog.action; //bark

```

As can be noticed, the change in `Animal` constructor is reflected on its instance `dog`. That's thanks to the prototype property of the `Function` object. 
However, is this property referencing the real prototype of a function constructor? Nope.

```javascript

    function Animal(name){
        name = this.name; 
    }
    Animal.prototype; // Object{ function Animal(name) } -- referencing the constructor structure
    Animal.__proto__; // function(){} -- referencing the constructor prototype

```

The **real prototype** of a constructor is the function while the **prototype property** is an object reflecting the constructor and it's used to update all 
the instances created by that constructor whenever the constructor it is modified (for instance by adding a property).
