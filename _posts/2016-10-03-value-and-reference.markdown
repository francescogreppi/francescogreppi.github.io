---
layout: post
title:  "Functions: passing values or references"
date:   2016-10-03 14:00:00 +0100
categories: Javascript
comments: false
---

In this post I'd like to describe the way values and references are passed as arguments to functions. I think sometimes
it might be confusing, so let's make it clear.

```javascript

    function increment(x){
        x+=1;
    };

    var b = 1;
    
    increment(b);
    
    console.log(b); //1

```

In the example above we pass the variable `b` in to the function `increment(b)`. The function increases of 1 the value we passed into the variable.
Surprisingly, when we `console.log` the variable `b` the value is still 1. Why?

Javascript handles the variables we pass as arguments to a function **by value**, which means that what we pass to the function is the actual value of the variable,
but not the variable itself, which is not passed to the inner scope. That's why when we `console.log(b)` in the code above we are still referencing the var b with its original value,
not the one computed inside the scope of `increment()` function.

If we would have wanted to see a 2 as the result in the console, then the code should have looked like the following.

```javascript

    function increment(x){
        x+=1;
        return x;
    }

    var b = 1;

    b = increment(b);

    console.log(b); //2

```

Notice that here the value needs to be returned from `increment()` and then assigned to `b` so that the original value is overwritten.
Let's have a look at a second example.

```javascript

    function multiplyProperty(x){
        x.value *= 2;
    }

    var b = {value:3};

    console.log(b.value); //6

```

After the previous examples we might expect to see 3 as a result of this computation. Instead we have 6. What's happening here? Javascript handles differently primitives and objects when 
passing them as arguments to a functions. In fact, primitives are passed **by value** and objects are passed **by reference**, meaning that any property of the object is accessible within the function.

That's all for now, hope you have enjoyed.
