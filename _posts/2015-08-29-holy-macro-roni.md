---
layout: post
title: Holy Macro-roni!
categories: []
tags: []
published: True

---

![Homac]({{ site.baseurl }}/assets/images/homer_macaroni.gif)

### We want more
Recently, I've been learning more about how to get the most out of our favorite programming languages. Everyone knows that JavaScript in particular can quickly become unweidly and unsightly with just a bit of nesting, callbacks, etc, and many developers have already taken into their own hands to come up with solutions to produce more elegant, yet just as effective code from the natural (often referred to as "vanilla") language. One example of this is [Coffeescript](http://coffeescript.org/)- a language that compiles into JavaScript, but is much more aesthetically pleasing to work with.

### Enter, macros
As great as these other languages are, there's also another really nifty solution- creating macros. A macro is simply a piece of code that parses and transforms the code around it. Like most concepts in programming, this is a very basic idea, but also incredibly powerful when you consider just how it can help push a language forward. In this post, I will cover an example of how to utilize [Sweet.js](http://sweetjs.org/)- a library for creating macros in JavaScript- to create your own JavaScript operator!

### The awesome "Rocket operator"
The code snippets from this blog are taken from an excellent Sweet.js course on [Plurlsight](http://www.pluralsight.com/courses/sweet-js-get-started), which I'd highly recommend checking out if you like what you see here. What we're going to do is create a new operator to clean up the normal boilerplate code we'd expect to see in a regular JavaScript promise.

Here, we see a normal JavaScript promise (disregard the $ followed by numbers, that's a Sweet.js thing we'll cover later). This is a basic example, but this syntax can get messy quick.

![promise]({{ site.baseurl }}/assets/images/promise.png)

Now, we can create a macro to take the place of all that syntax, and still have it be incredibly expressive!

![rocket macro]({{ site.baseurl }}/assets/images/rocket_macro.png)

And here is your new "Rocket" operator in action!

![rocket operator]({{ site.baseurl }}/assets/images/rocket_operator.png)

### Niceee. But why use macros instead of XYZ?
With some basic macro use, you have the power to extend your favorite programming language. Read about an awesome ESXX feature/function that you wish were available today?? PutAMacroOnIt

### Resources
[Case-based Macros with Nick Balestra]( http://nick.balestra.ch/2015/sweetjs-case-macros-for-javascript/ )

[Rule-based Macros with Luke Savage]( http://lukesavage.me/technical/2015/08/29/sweetjs-and-rule-based-macros/ )

[Macro Hygienity with Zach Sebag]( http://zachsebag.com/2015/08/29/losing-your-hygienity.html/ )

[Pluralsight Sweet.js Tutorial]( http://www.pluralsight.com/courses/sweet-js-get-started )
