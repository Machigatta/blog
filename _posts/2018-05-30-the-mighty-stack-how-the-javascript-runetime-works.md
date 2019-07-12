---
layout: post
title:  "The mighty Stack – How the JavaScript-Runtime works"
date:   2018-05-30 18:05:19 +0200
categories: JavaScript, Workflow
---
Understanding the JavaScript-Runtime might be one of the most helpful things you could learn. Fully understanding the workflow of our applications might get you some new ideas and open up new ways of developing new applications or even help us on refactoring our old code.

The language JavaScript is amongst other things described as a one threaded language. But what does this mean for us and our application? At first we have to think about the term “Thread”. The term is used to describe a sequence of actions being executed after another, it is processing one command at a time. Many other languages are supporting multithreading, which would allow us to execute multiple commands at a time.

Now knowing the term “Thread”, you might be able to conclude how our JS-Runtime works as well. One command at a time.

## The mysterious stack
Let’s use a simple JavaScript code to make clear how it works and how the stack is handled.

```js
function getName(){
    return "Machigatta";
}
 
function printMyName(){
    var name = getName();
    console.log("Hi, my name is " + name);
}
 
printMyName();
```

So following this code-snippet we fill our stack like it executes it. So it have to look like this, from left to right (main symbolizes the initialization of the code):

![JS-Stack ><](/assets/blog_images/2018/stacking-1.png "Stack")

Following this idea, every call of a function is added at the top of the stack if the runtime is waiting for a return value. If a value or nothing(function ended) is returned, the call is popped from the top of the stack.

If you have trouble following this stack idea, there is another way of visualizing the stack via your browser as well. For this little trick, we will modify our example code and simplify it:

```js
function boy(){
    throw new Error('Triggered');
}
 
function hell(){
    boy();
}
 
hell();
```

If we’re going to execute this in our browser-console, we get, through triggering our error, a beautiful way of our stack as well:

![Error-Stack ><](/assets/blog_images/2018/errorStack.png "Error-Stack")

## Single threading brings errors
Needles to say, that there is a possibility of breaking this stack by overflowing it and returning the function by itself all the time:

```js
function stack(){
    return stack();
}
 
stack();
```
Even though this error is described as Uncaught RangeError: Maximum call stack size exceeded in your console, you might know this error in some other terms: Stack Overflow.

## It blocks!
The major problem of executing one task after another is finished, is that it waits. But why is that a problem? It’s simple… because JavaScript is executed in our browsers. Long running functions are blocking the browser, till the execution is finished. Again, let’s edit our code to our need.

```js
function counter(){
    console.log("checking");
    var i = 0;
    do {
       i += 1;
     
    } while (i < 2000000000)
 
    return "finished";
}
 
var callOne = counter();
var callTwo = counter();
var callThree = counter();
 
console.log(callOne);
console.log(callTwo);
console.log(callThree);
```

So while executing this code we will wait several seconds, because each counter is taking around one second to run through. As a summary this code will run around three seconds. You can go on any website(google for example), open your local browser-console and try executing this code. Now try pressing any buttons on the site you are executing this. You won’t be able to do anything for three seconds.

## There are solutions out there
Not everything is as bad as it seems in the first place. The solution is called Concurrency and Event Loop. While this is a bit too much for this post we will handle it in another one.