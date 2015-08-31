---
layout: post
title: Holy Macro-roni!
categories: []
tags: []
published: True

---

# Create Your Own JavaScript Operator using Macros

![Homac]({{ site.baseurl }}/assets/images/homer_macaroni.gif)

### We want more
Recently, I've been learning more about how to get the most out of our favorite programming languages. Everyone knows that JavaScript in particular can quickly become unweidly and unsightly with just a bit of nesting, callbacks, etc, and many developers have already taken into their own hands to come up with solutions to produce more elegant, yet just as effective code from the natural (often referred to as "vanilla") language. One example of this is [Coffeescript](http://coffeescript.org/)- a language that compiles into JavaScript, but is much more aesthetically pleasing to work with.

### Enter, macros
As great as these other languages are, there's also another really nifty solution- creating macros. A macro is simply a piece of code that parses and transforms the code around it. Like most concepts in programming, this is a very basic idea, but also incredibly powerful when you consider just how it can help push a language forward. In this post, I will cover a basic example of how to utilize [Sweet.js](http://sweetjs.org/)- a library for creating macros in JavaScript- to create your own JavaScript operator!

### The awesome "Rocket operator"
The code snippets from this blog are taken from an excellent Sweet.js course on [Plurlsight](http://www.pluralsight.com/courses/sweet-js-get-started), which I'd highly recommend checking out if you like what you see here. What we're going to do is create a new operator to clean up the normal boilerplate code we'd expect to see in a regular JavaScript promise.

Here, we see a normal JavaScript promise (disregard the $ followed by numbers, that's known as "variable hygiene" which Zach does a great job explaining [here](http://zachsebag.com/2015/08/29/losing-your-hygienity.html)). This is a basic example, but this syntax can get messy quick.

![promise]({{ site.baseurl }}/assets/images/promise.png)

Now, we can use Sweet.js to create a macro to take the place of all that syntax, and still have it be incredibly expressive! Here's what's happening:

- The '=>' represents a macro that does the same thing as a function "fat-arrow" from Coffeescript or ES2015 (aka ES6).

- The '==>' represents our new macro that condenses promise boilerplate.

- The '12' refers to our new operator's precedence (you can assign any value you wish). Every operator has their own precedence, which you can find [here](http://sweetjs.org/doc/main/sweet.html#operator-precedence).

- The { $l, $r } represents whatever will be found to the immediate left and to the right of our '==>' operator.

- The => #{ $l.then($r) } represents our expected output. In this case, we'll have the code from the left of the operator, followed by a call to .then, passing the code from the right of the operator as an argument to .then.

![rocket macro]({{ site.baseurl }}/assets/images/rocket_macro.png)

And here is your new "Rocket" operator in action! Ain't it pretty?

![rocket operator]({{ site.baseurl }}/assets/images/rocket_operator.png)

### Niceee. But why use macros instead of XYZ?
With some basic macro use, you have the power to extend your favorite programming language. Read about an awesome ES20XX feature/function that you wish were available today?? PutAMacroOnIt

### Resources
If you want to learn more about macros and Sweet.js, here are a few awesome blog posts to help you get started!

[Case-based Macros with Nick Balestra]( http://nick.balestra.ch/2015/sweetjs-case-macros-for-javascript/ )

[Rule-based Macros with Luke Savage]( http://lukesavage.me/technical/2015/08/29/sweetjs-and-rule-based-macros/ )

[Macro Hygienity with Zach Sebag]( http://zachsebag.com/2015/08/29/losing-your-hygienity.html )

[Pluralsight Sweet.js Tutorial]( http://www.pluralsight.com/courses/sweet-js-get-started )
