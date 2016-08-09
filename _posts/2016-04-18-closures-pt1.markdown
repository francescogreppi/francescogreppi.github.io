---
layout: post
title:  "Closures pt.1 - the lexical environment"
date:   2016-04-18 14:00:57 +0100
categories: Javascript
---

Since I started programming in Javascript I always knew that functions can access their outer scope. It's a pattern you can easily identify when playing a bit with functions in the console.
This pattern is called closure and there's much more to know about it than the simplistic definition given above.

# The lexical environment #

To understand what is a closure I first have to introduce the lexical environment.

> A lexical environment defines the association of identifiers to the values of variables and functions based upon the lexical nesting structures of ECMAScript code.

What is cool about the lexical environment is that it consists of two components:

* the **environment record** where are listed all the bindings of variables/functions to their values
* the **reference** to the outer scope

Look at the example:


```javascript
	var a = 1;

	function foo(){
		var b = 3;
	}
```

We have two lexical environments here:

* one is related to the **global context**
* the other one is related to the `foo()` function

Let's look at the snippet below to better understand it.

```javascript
// global context environment (window object if you are on the client side)
globalEnvironment = {

	environmentRecord : {

		// here we have built-ins such as object and array
		Object: function,
		Array: function,
		// etc..
		// and we have bindings
		a:1,
		foo: function

	},

	// nothing external to the global scope
	outer: null
};

// environment of the foo function
fooEnvironment = {
	environmentRecord: {
		// bindings
		b:3
	},

	// here we reference the global scope as the outer environment
	outer: globalEnvironment
}
```

When a function is **created** it is associated to the lexical environment to which it belongs via an hidden property called `[[Scope]]`.

When a function **runs** it creates its own `[[Scope]]` and thus its own lexical environment. Additionally, as we've seen, a reference to the original lexical environment is created too.

Look at this example.

```javascript
// here we have a global scope as before, with its own environment record
// where the binding is a:'Hellow' and splitHello: function

var a = 'Hellow';

function splitHello(){

	// here we have the function scope, with its own ENVIRONMENT RECORD
	// AND with the REFERENCE to the outer scope
	// remember, the function scope is created WHEN the function RUNS
	// we are also using the built-in split() function, from the global scope
	// but where is a? and how split() is accessed?

	var b = a.split();
}
```

The `a` variable and `split()` built-in function are not found within the function scope but, thanks to the lexical environment reference, the function is able to look for them into the outer scope, finally using them for its own purpose. Cool uh?

So far I discussed the concept of lexical environment, let's now extend it to the case of nested functions where we have **chains of lexical environments**. Click [here](/javascript/2016/04/20/closures-pt2.html) to continue.
