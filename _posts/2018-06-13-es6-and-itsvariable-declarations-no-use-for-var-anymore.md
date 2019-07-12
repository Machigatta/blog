---
layout: post
title:  "ES6 and its variable-declarations – no use for var anymore"
date:   2018-06-13 15:50:00 +0200
categories: Explanation
---
Some years have gone by since the release of the new ECMAScript Standard 6 and there are still many people out there using __var__ to declarate variables in JavaScript. I understand that even after these years, there might be some versions of browsers which do not support the __let__ and __const__ declaration. But if you’re building an application and you’re concerned about your security, be warned, there are some new ways of declaration in town.

## The difference between “function scope” and “block scope”
Ok, at first let me declare what kind of declaration method uses what kind of scope.

function scope => __var__
block scope => __let__


But what’s the difference in their usage. Let’s be clear, there might be thousands of examples out there with more complex explanations where __let__ works different from __var__. For my part, I just want to tell you the basic difference and show you your new possibilities.

If you’ve learned programming with a language like C#, C++ or Java you might got used to block scope and didn’t even know something like _*function scope*_ might exist. If you look at the words themselves you might get the idea behind it. A scope is mainly the frame to work in and the name before defines the boundaries. In javascript the function boundaries are indicated, who would have known, through the function-tag (_*there are some shortcuts for defining a function too*_). A block on the other side is everything, every time defined between the curly brackets { and }. Let’s break it down and say you’re writing a simple function which is counting down from 1 to 100. Let’s use the declaration __var__ for this example and execute this one in our console.

```js
function start(){
 
    //Console: undefined
    console.log(i);
    for (var i = 1; i < 10; i++) {
        //Console: 1 - 10
        console.log(i);
    }
    //Console: 100
    console.log(i);
    
}
```

I already added some comments to show you how this would work. On the first look, this seems pretty good, doesn’t it? We log our variable and it’s undefined, seems fine on the first look. We declare our variable in our loop-block and the right logs are printed. But why is our last console.log printing a 100? That’s the main problem behind the usage of var. To make clear why this is messed up, we have to look at the snippet after the interpreting of our code has started.

```js
function start(){
    var i;
    //Console: undefined
    console.log(i);
    for (i = 1; i < 10; i++) {
        //Console: 1 - 10
        console.log(i);
    }    
    //Console: 100
    console.log(i);
}
```

JavaScript has put our variable declaration at the top of the function. The method is called __hoisting__ is pretty much the reason behind the whole trouble. The declaration method __let__ instead will stay only in the block itself.

```js
function start(){
    //Console: Would result in an error
    console.log(j);
    for (let j = 1; i < 10; i++) {
        //Console: 1 - 10
        console.log(j);
    }    
    //Console: Would result in an error
    console.log(j);
}
```

This also explains the _*undefined*_ result:

```js
function start(){
    //Console: Would result in an error
    console.log(j);
    
    let j;
    
    //Console: undefined
    console.log(j);
    
    j=10;
    
    //Console: 10
    console.log(j);
}
```

Of course this doesn’t mean that you should edit all your past projects and use the declaration method __let__. It means that you have to be aware of the usage and workflow of them both.

## What about const?
To define the usage, you have to get used to the way it works in the first place. The keyword const stands for the name “constant”, means it’s value should always stay constant. BUT after we are still working with JavaScript, there are some small exceptions to this behavior.

```js
//often seen, because many functions are created to not be touched afterwards
const go = function start(){
    
    const simpleConst = "My name is Machigatta";
    //Console: My name is Machigatta
    console.log(simpleConst);
    
    //Would result in an error (Uncaught TypeError: Assignment to constant variable.)
    simpleConst = "My name is Eduard";
    
    
    const objConst = {name: "Machigatta"};
    //Console: Machigatta
    console.log(objConst.name);
    
    //Would result in an error (Uncaught TypeError: Assignment to constant variable.)
    objConst = "Eduard";
    
    //Console: Eduard
    objConst.name = "Eduard";
    console.log(objConst.name);
    
    //Console: Machigatta
    objConst.old_name = "Machigatta";
    console.log(objConst.old_name);
    
};
 
go();
```

Even though the value itself is basically constant, there are some exceptions to it. Like changing the property of an object ob even adding a new property.

## Note:
> If you really have to edit your variable, use let. Otherwise use const. Sadly var is dead for us now.