---
layout: post
title:  Weekend Groove
date:   2014-08-24
categories: jekyll update
tags: sass responsive mobile koans korra 
---

Living in the middle of nowhere (Manteca, CA) definitely has its pros and cons. I really miss being in the city, exploring new places, taking in the awesome view anywhere you go, being able to set up shop and get work done at lively coffee shops, or just sitting and people watching over a Super Duper burger. Unfortunately, the commute out there is incredibly involved (train, bus, bart, spanning a few hours), but at least it's an option for when I need to be out there. That being said, I really can't complain- I'm saving a ton of money living this far out, which will be very useful in the coming months for my more substantial investments/purchases. Also, one big benefit of living where there's pretty much nothing to do is that it's allowing me to focus on the studying/development that I put off/'didnt have time for' before leaving the city. It's been a slow and steady process, and though still very early in it, I really enjoyed this weekend indoors, diving into old code and learning a few new things here and there. Stella's gettin her groove back!

Speaking of 'groove' and missing the city, something that always helps me get in the zone when I'm working is a great playlist, and I impulse bought tickets to 2 concerts in the city, hoping to add 1 more soon. Excited for Snakehips and Kygo, still trying to come up on The Weeknd tix! Man, great music right there.

There was quite a bit of code looked over this weekend, and I'm not exactly sure how I'll structure this post or which I'll decide to emphasize, but it's late and I have work in the morning, so I'll just get as many coherent notes down before the eyes get bloodshot.

#Koans
I tried to make a dent in the Koans so I can wrap up the whole thing by the end of next week. Currently I'm at 170/282 exercises completed, and though most thus far have been fairly trivial, there have definitely been a few that had me thinking for a bit. The topics covered included: constants, inheritance, control statements, exceptions, iteration, keyword arguments, raising errors, methods, and default values. Here are a few notes that I found helpful:

{% highlight ruby %}
# QUESTION: Now which has precedence: The constant in the lexical
# scope, or the constant from the inheritance hierarchy?  Why is it
# different than the previous answer?
# I had to think about this one for a bit.. I think it has something to
# do with how the Oyster class is defined in MyAnimals, and to confirm
# this assumption, I found this great answer on Stack Overflow via @bowsersenior
# I was just pondering the very same question from the very same koan. I am no expert
# at scoping, but the following simple explanation made a lot of sense to me, and maybe it will help you as well.
# When you define MyAnimals::Oyster you are still in the global scope, so ruby has no knowledge of the LEGS
# value set to 2 in MyAnimals because you never actually are in the scope of MyAnimals (a little counterintuitive).
# However, things would be different if you were to define Oyster this way:

class MyAnimals
  class Oyster < Animal
    def legs_in_oyster
      LEGS # => 2
    end
  end
end

# The difference is that in the code above, by the time you define Oyster, you have dropped into the scope
# of MyAnimals, so ruby knows that LEGS refers to MyAnimals::LEGS (2) and not Animal::LEGS (4).

###

# Raising errors inside triangle(a,b,c)...
if a <= 0 || b <= 0 || c <= 0
	raise TriangleError, "No side can be less than or equal to zero."
end
if ((a+b) <= c) || ((a+c) <= b) || ((b+c) <= a)
	raise TriangleError, "No side can be greater than or equal to the sum of the other two."
end

###

#Exceptions
class MySpecialError < RuntimeError
end
 
# Different classes inherited with RuntimeError
def test_exceptions_inherit_from_Exception
 assert_equal RuntimeError, MySpecialError.ancestors[1]
 assert_equal StandardError, MySpecialError.ancestors[2]
 assert_equal Exception, MySpecialError.ancestors[3]
 assert_equal Object, MySpecialError.ancestors[4]
end

# Anything in 'ensure' block is ALWAYS run
def test_ensure_clause
  result = nil
  begin
    fail "Oops"
  rescue StandardError
    # no code here
  ensure
    result = :always_run
  end

  assert_equal :always_run, result
end

###

#Flow Control
# break statement can return values
def test_break_statement_returns_values
  i = 1
  result = while i <= 10
    break i if i % 2 == 0
    i += 1
  end

  assert_equal 2, result
end
{% endhighlight %}

There were even more koans from the weekend, but this is good for now.

#CodeSchool

This weekend was also a great opportunity to dive back into some CodeSchool courses. The last I recall doing was their javascript course, which I enjoyed a lot, and since then they've put out some really great content. I completed the [Journey into Mobile][csmobile] course, and while I learned a few useful things, it's probably not one of my favorite courses on CS. It teaches the very basics of mobile, adaptive, and responsive design, and at the very least it should be enough for me to continue my own learning with a few other resources. I also started the [Assembling Sass][sass] course, which I'm really enjoying so far. I've been wanting to learn Sass for a while, and was able to effectively use it in the project that I'll talk more about in a bit. Even though I've only gone over the first few levels, I really like how it's already cleaning up the stylesheets! Perdy code :)

#Craigs Jr gets Sassy and Responsive
This old project has haunted my coding nightmares for a while now. Not for a matter of difficulty (it's one of the first exercises given to students in this phase), but rather just for lack of dedicating time to complete what I wanted to with this review. Since I've done this project a few times before (when I was a student, and then helping newer students), I wanted to use this time to introduce other tools, and those include Sass and media queries to make it responsive. I only implemented a very basic look for this project, but I'm actually really happy with the progress. While getting this to work, I've accumulated a few nifty tools, such as Sublime packages to compile sass files on save, and a browser tool to quickly switch to popular/custom viewports. Here's how the app responds to the iPhone 5/5s viewport:

![responsive craigs jr][craigsgif]

Nothing fancy, but surprisingly fun considering I haven't messed with CSS in forreeeverrrr. Looking forward to getting even Sassier! #ohsnap

#Misc notes
{% highlight ruby %}
# ActiveRecord
# The :include option allows you to name associations that should be loaded alongside with the models.

# SecureRandom#hex
# generates a random hex string
# used for edit_key in craigslist, would typically be emailed to User

# Many of your routes are now dependent on sessions, which will cause your tests to break. But that's okay! 
# You can simply fake a user's session using rack-test, as the 'get' and 'post' method take an optional 
# third argument which corresponds to the rack #environment hash, where information about sessions is stored.
# It would look something like this:
it "should create a new post" do
  fake_params = { title: "some title", content: "some content" }
  fake_session = { 'rack.session' => { user_id: 20 } }
  expect{
    post '/some_route', fake_params, fake_session
  }.to change{ Post.count }.by(1)
end
{% endhighlight %}

It's getting to be that hour (curr 1:48am). It's been a fun marathon this weekend, but time for sleep. Til next time...

![asleep at the comp][sleep]

[csmobile]:	https://www.codeschool.com/courses/journey-into-mobile
[sass]:			https://www.codeschool.com/courses/assembling-sass
[craigsgif]:http://g.recordit.co/IScpHU8Fuo.gif
[sleep]:		http://24.media.tumblr.com/tumblr_ldlmg7USlL1qfo270o1_r1_500.gif