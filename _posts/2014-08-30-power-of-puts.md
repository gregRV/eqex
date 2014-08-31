---
layout: post
title:  Power of puts!
date:   2014-08-30
categories: code
tags: debugging koans 
---

It's been a few days since the last entry, and I've just been trying to stay productive with some studying, as well as some life events that seem to be going 999 mph. It's exciting/exhausting, and really makes me notice how fast this year has flown by, as well as how quick it'll be over. I have something big planned later this year (I know, I keep teasing this), and am really excited to share more details as all the details are finalized.

So, what have I been reviewing? Here are a few areas that I've been putting in some time:

**[Code School 'Assembling Sass' course][cs sass 1]**

Even though there are things I'm way more interested in reviewing than CSS, I've had a surprisingly good time learning more about Sass. Even with the basics covered in the tutorial, I've noticed how much things like nesting and variables make the stylesheets so much more readable. Also, the use of mixins, extends, directives, and functions add an awesome level of dynamic behavior, and I'm particularly interested in media queries and other mobile-first design tools. There's a [part 2][cs sass 2] to this course that I'm looking forward to starting, but I'd like to use the first level's teachings in a small project to solidify the concepts.

**Koans**

I've done a few koans since the last post, but this code snippet is from the most recent and one that I wanted to comment on a bit. The idea was to create a method that appropriately *scored* an array passed as an argument based on a set of rules. At first glance, I thought I'd spend at most 10 minutes on this exercise, but the 80/20 rule was very alive here. Majority of the time was spent getting the final few tests to pass, and I realized after a lot of re-reading my code, that I could have greatly benefited from spending just a bit more time whiteboarding/pseudocoding before starting the exercise. By not properly planning, I ended up with the gross ~70 line method seen below. All tests pass, but man... look at that thing :/
I know there was logic that I wanted to extract into helper methods, but as the time passed, I just wanted to figure out why the tests weren't passing, so I stuck with my brute-force code with scant commments to hopefully make sense of things if someone were to ever look over this work.

{% highlight ruby %}
def score(dice)
  return 0 if dice.empty?

  # PREPARING DATA
  ################

  # score to be returned
  score = 0

  # counters representing the amount of 1's and 5's in array
  ones  = 0
  fives = 0

  # iterate through array to check amount of 1's and 5's
  dice.sort!
  dice.each do |elem|
    if elem == 1
      ones += 1
    end
    if elem == 5
      fives += 1
    end
  end

  # check for other triples, store in array for later calculation
  curr_num = dice[0]
  counter = 0
  triples = []
  dice.each do |elem|
    if curr_num == elem
      counter += 1
      if counter == 3
        triples << elem
        counter = 0
      end
    end
    curr_num = elem
  end

  # get rid of 1's and 5's in triples array
  triples.delete(1)
  triples.delete(5)


  # START ADDING TO SCORE
  #######################

  # this needs to be first! check for group of 1's
  if ones >= 3
    score += 1000
    ones -= 3
  end

  if fives >= 3
    score += 500
    fives -= 3
  end

  # adds 100 to score for each 1 in dice
  score += 100*ones if ones > 0

  # adds 50 to score for each 5 in dice
  score += 50*fives if fives > 0

  # add other triples
  unless triples.empty?
    triples.each do |elem|
      score += elem*100
    end
  end

  # return final score
  score
end
{% endhighlight %}

I wanted to share this just to remind myself to not speed through the essential planning stages, and even after everything *looked* like it should work, a simple debugging practice of *puts*'ing out values throughout the method allowed me to find that I had overlooked an incredibly simple and bone-headed error- I assigned a hard value when I actually wanted to decrement and reassign a value, like so:

{% highlight ruby %}
# ORIGINAL
ones=-3
fives=-3

# INTENDED
ones -= 3
fives -= 3

# :(
{% endhighlight %}

Lesson learned.

**Other Fun Stuff**

I also installed a Sublime [Jekyll package][jekyll package] for funsies, and it comes with a ton of useful commands and snippets. I'm only using a few currently, but as I start exploring more how to get the most out of a Jekyll site, I'll definitely be utilizing these on the regular.

**What's Next**

With the rest of the weekend (tonight and tomorrow), I'm planning on building a Twitter clone, focusing on what I've learned from the Sass tutorials for a mobile-first design. It'll also be great reviewing some [ActiveRecord associations][ar]- specifically Self Joins for things like "Followers"/"Following"/"Favorites", etc. We'll see how much I can bang out in the next 30 hours or so, as I have some errands I need to run tomorrow as well.

Stay tuned :D

[cs sass 1]:			https://www.codeschool.com/courses/assembling-sass
[cs sass 2]:			https://www.codeschool.com/courses/assembling-sass-part-2
[jekyll package]: https://sublime.wbond.net/packages/Jekyll
[ar]:							http://guides.rubyonrails.org/association_basics.html#self-joins