---
layout: post
title:  "Closures pt.2 - nested functions"
date:   2016-04-20 10:00:57 +0100
categories: Javascript
comments: false
---

Ok, after the lexical environment I now have to explain myself nested functions, which is not that hard to get, I see it as a logical consequence of the lexical environment. Then I'll proceed to the core of closures.

So, the cool stuff of functions is that they can be nested into each other. This particularity generates the scope chain, which is a **chain of lexical environments**, all connected between them in a hierarchical structure.

Let's look at an example:

```javascript

	// here our lexical environment is the global scope
	// in a browser the global scope is the window object
	// a:5, sum:function

	var a = 5;

	function sum(){
		// this is the lexical environment of sum()
		// b:3, addition: function
		// reference: global lexical environment
		var b = 3;

		function addition(){
			//this is the lexical environment of addition
			// c: a+b
			// reference: sum lexical environment
			var c = a+b;
		}

		return addition;
	}

```

Thanks to this hierarchy, functions can access their outer scope, straight to the global scope. Let's now have a look at [closures](/closures-pt3/).