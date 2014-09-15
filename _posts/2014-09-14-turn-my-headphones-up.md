---
layout: post
title:  Turn My Headphones Up
date:   2014-09-14
categories: study fun
tags: music rails dbc
---

Since my last post, I've been getting to a better place in life and starting to feel better about personal events, which feels really good. I was pretty depressed and anxious for the past while, but it was necessary, and now I'm looking forward to the future and making the most of every day.

This past Friday I was able to visit San Francisco for the first time in a few months, and it couldn't have been at a better time. It was great seeing familiar faces at the office, seeing how much some of the students have grown since we last spoke, and I really missed just walking around the city in this perfect weather. It was incredibly refreshing, and just what I needed.

Coincidentally, I arrived on a Friday that was also a graduation day at Dev Bootcamp, which is always a fun time. I got to see a batch of awesome final projects (shout out to the '14 Mule Deer), attend the grad party and catch up with coworkers, and after went to see Porter Robinson live at the Bill Graham Civic Center!

![Porter Robinson - WORLDS Tour '14][porter rob]

*OMGAAAAHHHHHH THAT SHOW.*

It was so incredibly fun, and I think a big part of it was just the act of going to a big social event on my own. I met a lot of awesome people, jumped around like a mad man to amazing music, and got footage that I must've watched a dozen times already. I'm making an effort to put myself into more of these types of situations and experience as much as I can, while I can. Speaking of which, more SF concerts in October!

As far as studying goes, I've been taking some time to dive into Rails 4. This has been great so far because it's been forever since I've worked in Rails, and I've definitely encountered several issues with the conventions as I remembered them (from v3.2.13), and what Rails currently asks for. Below are a few screenshots from the past session, which has mostly been a refresher on user authentication:

- - -
- - -

**Strong Parameters error**

![Strong Parameters error][strong params error]

**Simple fix via private method allowing assignment**

![Strong Parameters fix][strong params fix]

To further enforce this concept, here's the example from Edge docs:

{% highlight ruby %}
class PeopleController < ActionController::Base
  # Using "Person.create(params[:person])" would raise an
  # ActiveModel::ForbiddenAttributes exception because it'd
  # be using mass assignment without an explicit permit step.
  # This is the recommended form:
  def create
    Person.create(person_params)
  end

  # This will pass with flying colors as long as there's a person key in the
  # parameters, otherwise it'll raise an ActionController::MissingParameter
  # exception, which will get caught by ActionController::Base and turned
  # into a 400 Bad Request reply.
  def update
    redirect_to current_account.people.find(params[:id]).tap { |person|
      person.update!(person_params)
    }
  end

  private
    # Using a private method to encapsulate the permissible parameters is
    # just a good pattern since you'll be able to reuse the same permit
    # list between create and update. Also, you can specialize this method
    # with per-user checking of permissible attributes.
    def person_params
      params.require(:person).permit(:name, :age)
    end
end
{% endhighlight %}

- - -
- - -

**Verifying Authentication works in Console**

![Rails console][rails c]

*false* is returned when I pass in 'bad' as the password (correct password is 'fifpass', as seen in 2nd query). Correct credentials returns the the appropriate User record.

- - -
- - -

**email_field_tag arguments**

![email_field_tag][email_field_tag]

Just a reminder here. I was forgetting the *value* argument before.

![form_tag][form_tag]

![Creating a Session][session create]

Basic authentication flow, with *notices* used to show if the session was created, or if there was an error finding the User with the provided credentials.

- - -
- - -

So far, all basic stuff, but feels good to shake off the rust. There are a few things that I want to implement with this Twitter clone, but I don't want to spend more time here than necessary. Hopefully with this week's work schedule, I'll be able to do everything I want to with this project by Wednesday, then it's on to the next one.

So for the rest of the night, crank the headphones, vibe off that Porter, Kygo, Meghan Trainor, and get down with some Rails goodness!

TURN MY HEADPHONES UP!

![turn my headphones up][headphones]

[porter rob]:						http://bit.ly/1qPcGbc
[strong params error]:	http://i.imgur.com/8eY6NiO.png
[strong params fix]:		http://i.imgur.com/dI6uMNs.png 
[rails c]:							http://i.imgur.com/Ljr8AoH.png
[email_field_tag]:			http://i.imgur.com/hjMz7Eg.png
[form_tag]:							http://i.imgur.com/F4T7p6w.png
[session create]:				http://i.imgur.com/Q6wZ5Fz.png
[headphones]:						http://25.media.tumblr.com/tumblr_m0ne28vdhK1qifeu2o1_500.gif