---
layout: post
title:  "Closures pt.3 - the concept"
date:   2016-04-21 11:00:00 +0100
categories: Javascript
comments: false
---

Ok, thanks to my exploration of lexical environment and nested function I should have now enough knowledge to explain what a closure is. I found a clear example on [StackOverflow](http://stackoverflow.com/questions/111102/how-do-javascript-closures-work?page=1&tab=votes#tab-top) which I'm going to report below.

```javascript

	// global scope

	function sayHello(name) {
		// local scope
		var text = 'Hello ' + name; 

		var say = function() { 
			// local scope
			console.log(text); 
			}
			return say;
	}

	var say2 = sayHello('Albert');
	say2(); // logs "Hello Albert"

```
So, here's what's happening, read it slowly: 

* we are declaring a function `say()` within a function `sayHello()`;
* than we create the variable `say2()`, and we associate it to the `sayHello()` function;
* `sayHello()` is returning `say()`, which is an anonymous function that can access the local variable of `sayHello()` and console log it;
* we can imagine the association like that `var say2 = function(){console.log(text)}`;

Let's stop a second on a really interesting point: in most of the traditional programming languages when a function is returned all of its local variables are destroyed. That's not happening in Javascript. In fact the local variables are kept even after the function has returned.

So what happens at the end is that when we call `say2()`, even if `sayHello()` has returned already in the line of code above, we are still able to console log 'Hello Albert'.

That's the concept of closure. I'm still a bit confused, but let me report below the two lines of definition provided by [MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Closures).

> Closures are functions that refer to independent (free) variables. In other words, the function defined in the closure 'remembers' the environment in which it was created.

Yes, that starts to make sense now. A closure is a function that can remember its environment, basically the lexical environment where it was created. 

So, when we declare `say2`, we are creating a special object that combines a function (`function(){console.log(text)}`) and the environment in which that function was created, in this case the lexical environment of `sayHello()`, where we have the variable `text` and the parameter `name`.

That's all for now, hope you have enjoyed.

Back to [home](/).