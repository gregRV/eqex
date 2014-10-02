---
layout: post
title:  Red, Red, Refactor
date:   2014-10-1
categories: study
tags: rspec testing
---

OK! Let's see if I can get a (semi) coherent blog post within 45 minutes. That may seem like way too long to most, but I tend to linger on writing, even when it's mostly just a reminder to myself. LESSGOOO

After Tweetito, I started working on a simple ToDo Rails app. This obviously is nothing worth writing home about, but what I really wanted to challenge myself by approaching the entire process with Test-Driven Development. I didn't get much exposure to TDD before, and though I completely understand the concept, I've always had a hard time implementing it in a few other small projects. I've discovered that the main source of confusion for me is understanding exactly *where* to start testing. I've gone through countless blogs, videos, and exercises, and have read a few common answers. Some say that tests are literally the **very first** lines of code that you write. Some say to start specifically with **integration tests** so you're only building what's necessary as a user. Other resources say start with your basic schema, and then write unit tests to flesh out your models. I guess choices are good, but sometimes they're a bit debilitating and confusing. I don't want to start bad habits, but I'll have to develop any habits first to determine if they're bad or not, and that simply means GET TO CODING, whether or not it's following someone's "best practice".

I will say, currently it makes sense to me to get started with a basic schema, then carry your TDD from there. If we were starting completely from scratch, it would seem the idea is to create a new test then migration for *every single field* in a table, which I have a hard time believing. If I know the very basic fields of a User, then I should be able to create effective tests for ensuring someone can "sign up" without a lot of unnecessary steps in the middle. This could be my own misunderstanding, but hey, that's the whole point in studying/practicing. I should have a better idea by the end of this exercise.

As usual, I've taken a few screenshots, and will add a small caption for each:

***
![new rails project][rails new]

This is just a reminder for creating a new Rails project without the default db and test settings.

***
![starting with rspec][rspec:install]

This is also a reminder for to create the spec directory with a new project (using RSpec)

***

![factory_girl and capybara][fg and capybara]

Nice brief example of the difference between standard Rails/RSpec testing and incorporating FactoryGirl and Capybara!

***
![rails generate controller without test files][no test framework]

Screenshot from Railscast, generating a controller without their respective test directories/files (if you'd rather diy).

***
![no more db:test:prepare][maintain_test_schema]

Rails 4.1+, no more db:test:prepare!

***
![require rails_helper][rails_helper]

![rails_helper vs spec_helper][rails_helper vs spec_helper]

These two together fried my brain for way too long. The Rails path helper methods and the capybara methods weren't being recognized in my tests because I was requiring 'spec_helper', as I've been used to doing in all other projects before Rails 4. Now, we require 'rails_helper' in our spec files instead, and 'rails_helper' requires 'spec_helper'. Above is a brief answer as to why.

***
![finally, a single passing test][sigh/wimper]

Battlescars for a little green dot.

***

Welp, it's now 8:00pm, and pretty much right on time! Pushing this post up, and getting back to work!

\#moargreendots

[rails new]: 						http://i.imgur.com/rb7tFwn.png
[rspec:install]: 				http://i.imgur.com/hB64U8O.png
[fg and capybara]: 			http://i.imgur.com/8poyXH1.png
[no test framework]: 		http://i.imgur.com/DH7vlZS.png
[maintain_test_schema]:http://i.imgur.com/cO08eo1.png
[rails_helper]: 				http://i.imgur.com/JMdPteq.png
[rails_helper vs spec_helper]: http://i.imgur.com/x0XtsbU.png
[wrong helper]: 				http://i.imgur.com/ncb2AO0.png
[sigh/wimper]: 					http://i.imgur.com/LQaIMEN.png