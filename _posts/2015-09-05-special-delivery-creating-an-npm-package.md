---
layout: post
title: Special Delivery! Creating an npm Package
categories: []
tags: []
published: True

---

![Special Delivery]({{ site.baseurl }}/assets/images/package.jpg)

### Sharing is Caring

As developers, we rely heavily on libraries written by other members of the community on a daily basis. That's actually one of the best things (in my opinion) about being in this space- people are constantly pushing themselves to share knowledge, tools, and practices to help make others' workflows and daily lives that much easier. Some of these libraries come from huge companies like Google and Facebook, while other (just as popular) tools are contributed by small teams (or even individuals) who do this purely out of hobby. One of the best parts about this system is that creating and sharing your own package is made incredibly easy by npm, the node.js package management system. Who knows, maybe you'll author the next _(insert 20XX hawt.js framework)_!

### Let's Get Started

_This tutorial will assume you have basic skills with command-line and JavaScript_

This example package that we're about to create is simply responsible for exporting a single function that console log's a message.

#### npm init
From the command-line, enter `npm init`. This will guide you through a series of questions including: who is the author, package name, package description, etc. Don't worry about having all of these answers right away- you can always edit the file that this prompt generates. When you are done answering the questions, it will generate a `package.json` file for you will all the information you just entered. You're pretty much halfway there to publishing your package!

#### package.json & index.js
In `package.json`, you will see a property named `main`, which represents the entry-point, or main file to load in order to start using your package. By default, it will assign the name `index.js` (which you can always change).
Open `index.js` in your text editor, and we're going create a simple function to export, like so:

```
exports.myFunction = function() {
  console.log("Here's a message from your new module!");
};
```

#### npm login
From the command-line, run `npm login` which will either guide you through signing up for npm, or logging into your existing account.

#### npm publish
Once, you're logged-in, run `npm publish` from the same directory as your `package.json` file.

#### That's It!
If you visit the url "https://www.npmjs.com/~_your_user_name_", you can view your new package, including statistics like download count, GitHub issues, etc. And now, most importantly, people can use your new package by running the command `npm install _your_package_name_`.

**"How cool is that?!"** _(Rivers Cuomo voice)_

### Resources
[Pick Your Parsing with Nick Balestra](http://nick.balestra.ch/2015/pick-your-parsing/)

[Deep dive into deepEqual with Luke Savage](http://lukesavage.me/technical/2015/09/05/deep-dive-into-deep-equal/)

[Stop Worrying and Love TDD with Zach Sebag](http://zachsebag.com/2015/09/05/tape-or-how-i-learned-to-stop-worrying-and-love-TDD.html)
