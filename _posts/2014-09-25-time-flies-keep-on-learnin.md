---
layout: post
title:  Time flies! Keep on learnin'
date:   2014-09-25
categories: study fun
tags: rails
---

Whoa! I said I'd try to finish my current project last Wednesday (9/17), but I guess 8 days late is better than never! The review while building this simple Twitter clone has been great, and while there's so much more that I wanted to do, there comes a point in every project where you simply need to finish that last feature and move on. In this post, I'm going to skim through this project and share key points that I feel are worth reminding myself of later down the road. Hopefully I can write this up in a reasonable amount of time and get back to coding!

But first, a quick look at the transformation...

*Before...*

![before pic][before pic]

*After!*

![tweetito flow][tweetito gif]

I wanted to keep the design simple for now, and spend most of the time focusing on back-end familiarity. And boy, did that take some time. There are only a few models here, namely: User, Tweet, Retweet, but where I ended up spending a lot of time is trying to get fancy with ActiveRecord to use a single model to represent several other models. For instance, I wanted to use User to also represent Follower and Following ("a user has many followers, and a user can follow many other users (*followings*)"). There was a great RailsCast that I used to learn more about [Self-Referential Associations][railscast163], though it seemed to focus on a single self-referential association, and I was looking at having two (follower, following). The naming got a bit tricky, but I was able to get it working. This logic also applied to Tweet, because I wanted to use this to represent Retweets, which was easier.

Here are some code snippets and notes from Tweetito:

**ActiveRecord Associations**
{% highlight ruby %}
class User < ActiveRecord::Base
	# via StackOverflow
	# In Rails 4, :order has been deprecated and needs to be
	# replaced with lambda scope block. Note that this scope
	# block needs to be passed before any other association
	# options such as dependent: :destroy etc.
	has_many :tweets, -> { order(created_at: :desc) }
	has_many :retweets

	# Self-Referential Association, via RailsCast 163
	has_many :follows
	has_many :followings, :through => :follows
	has_many :inverse_follows, :class_name => "Follow", :foreign_key => "following_id"
	has_many :followers, :through => :inverse_follows, :source => :user

	# Adds methods to set and authenticate against a BCrypt password. 
	# This mechanism requires you to have a password_digest attribute.
	# Validations for presence of password on create, confirmation
	# of password (using a password_confirmation attribute) are
	# automatically added
	has_secure_password
end

#reminder for password_digest in table
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
    	t.string :username
    	t.string :email
    	t.string :password_digest
    	
      t.timestamps
    end
  end
end

class Follow < ActiveRecord::Base
	belongs_to :user
	belongs_to :following, :class_name => "User"
end
{% endhighlight %}

**Sass (nesting, mixins)**
{% highlight sass %}
.top-nav {
	@include box-sizing;
	width: 100%;
	padding: 5px;
	border-bottom: 1px solid $background-dark-2;
	font-size: 20px;
	background-color: $off-white;
	display: inline-block;

	.right {
		float: right;
		display: inline-block;

		a {
			margin-left: 20px;
		}
	}
}

@mixin headers-and-tweets {
	background-color: $off-white;
	border-radius: 15px 0;
	border-bottom: 1px solid $background-dark-2;
	border-right: 1px solid $background-dark-2;	
}

@mixin new-session-and-user-form {
	@include headers-and-tweets;
	padding: 10px 0;
}

// Helpful BOX-SIZING explanation here:
// http://css-tricks.com/almanac/properties/b/box-sizing/
// Used to keep the nav bar within the page.
// Without box-sizing, the link furthest right spilt off the page
@mixin box-sizing {
  -webkit-box-sizing: border-box; /* Safari/Chrome, other WebKit */
  -moz-box-sizing: border-box;    /* Firefox, other Gecko */
  box-sizing: border-box;         /* Opera/IE 8+ */
}
{% endhighlight %}

**Helper methods and before_actions**
{% highlight ruby %}
class ApplicationController < ActionController::Base
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :exception

  before_action :require_login

  helper_method :current_user, :logged_in?

  def current_user
  	@current_user ||= User.find(session[:user_id]) if session[:user_id]
  end

  def logged_in?
  	!current_user.nil?
  end

  private

  def require_login
    unless logged_in?
      flash[:error] = "You must log in first"
      redirect_to new_session_path
    end
  end
end
{% endhighlight %}

**Sessions**
{% highlight ruby %}
class SessionsController < ApplicationController
	skip_before_action :require_login, only: [:new, :create]

	def create
		# The line below works, and will return either false, or the User record
		# user = User.find_by(email: params[:email]).try(:authenticate, params[:password])

		# Here is a solution from RailsCasts using #authenticate, which is
		# an instance method provided by has_secure_password
		user = User.find_by(email: params[:email])
		if user && user.authenticate(params[:password])
			session[:user_id] = user.id
			redirect_to user, notice: "You've successfully logged in."
		else
			redirect_to root_path, notice: "Log in failed."
		end
	end

	def destroy
		session[:user_id] = nil
		redirect_to root_path, notice: "You've successfully logged out."
	end
end
{% endhighlight %}

**Restrict Access**
{% highlight ruby %}
# TweetsController
# User can not tweet from another user's page (or more specifically w/another user's ID)
def new
	@user = User.find(params[:user_id])
	if @user.id != current_user.id
		redirect_to user_path(current_user), notice: "Access forbidden"
	end
	@tweet = Tweet.new
end
{% endhighlight %}

**Eager-loading** (still needs some work...)
{% highlight ruby %}
def show
	# is this the correct way to eager load for a single record?
	@user = User.includes(:tweets).find(params[:id])
	# figure out good way to include :retweets as well!
end
{% endhighlight %}

**Forms for nested resources**
{% highlight erb %}
<div class="new-tweet-form">
	<%= form_for [@user, @tweet] do |f| %>
		<%= f.text_area :body, value: @tweet.body, size: "30x10", required: true %>
		<%= f.submit "Tweet" %>
	<% end %>
</div>
{% endhighlight %}

**CRINGE-WORTHY** *that nesting... :(*
{% highlight erb %}
<!-- LIST OF TWEETS -->
<div class="tweets-list">
	<% if @user.tweets.any? %>
		<h3>Tweets</h3>
		<% @user.tweets.each do |tweet| %>
			<a href="<%= user_tweet_path(tweet.user, tweet) %>">
				<div class="tweet">
					<li class="timestamp"><%= tweet.created_at.strftime("%m/%d/%Y") %></li>
					<li><%= tweet.body %></li>
				</div>
			</a>
		<% end %>
		</div>
	<% else %>
		<h3>No Tweets</h3>
	<% end %>
</div>
{% endhighlight %}

Here are a few misc. screenshots from various resources that I found useful during this project:

**Destroy a generated Controller and all associated files**

![rails destroy][rails destroy]

**has_many :through  VS  has_and_belongs_to_many**

![has_many through][has_many through]

**@mixin VS @extend (Sass)**

![mixin vs extend][mixin vs extend]

Well, that was fun (and exhausting!). I have a few ideas as to where I want to take my studying next, but for now, food and more coffee!

![mat zo][mat zo]

*currently listening to Mat Zo & Porter Robinson - "Easy"*

[tweetito gif]: http://i.imgur.com/XgI2PQv.gif
[railscast163]: http://railscasts.com/episodes/163-self-referential-association?view=comments
[rails destroy]:http://i.imgur.com/nGs2ZbW.png
[has_many through]: http://i.imgur.com/sFMEUjw.png
[mixin vs extend]:  http://i.imgur.com/rVbdTQi.png
[before pic]:   http://i.imgur.com/vKeG7j4.png
[mat zo]: http://33.media.tumblr.com/aecd180fd082e03f2ca3a896225b845b/tumblr_mur8dysr201qftn52o1_500.gif