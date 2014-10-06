---
layout: post
title:  más testing & misc goods
date:   2014-10-5
categories: study
tags: rspec testing
---

This first portion comes (mostly) from the review that I did with Ruby Koans. It's a bunch of notes and code snippets all mashed together, so I'll separate parts and throw in comments where necessary:

{% highlight ruby %}
# INHERITANCE

# using 'super' and concatenating
class BullDog < Dog
  def bark
    super + ", GROWL"
  end
end

def test_subclasses_can_invoke_parent_behavior_via_super
  ralph = BullDog.new("Ralph")
  assert_equal "WOOF, GROWL", ralph.bark
end

class GreatDane < Dog
  def growl
    super.bark + ", GROWL"
  end
end

def test_super_does_not_work_cross_method
  george = GreatDane.new("George")
  assert_raise(NoMethodError) do
    george.growl
  end
end

# ------------------------------------------------------------------

# MODULES

class AboutModules < Neo::Koan
  module Nameable
    def set_name(new_name)
      @name = new_name
    end

    def here
      :in_module
    end
  end

  def test_cant_instantiate_modules
    assert_raise(NoMethodError) do
      Nameable.new
    end
  end

  class Dog
    include Nameable

    attr_reader :name

    def initialize
      @name = "Fido"
    end

    def bark
      "WOOF"
    end

    def here
      :in_object
    end
  end

  def test_normal_methods_are_available_in_the_object
    fido = Dog.new
    assert_equal "WOOF", fido.bark
  end

  def test_module_methods_are_also_available_in_the_object
    fido = Dog.new
    assert_nothing_raised do
      fido.set_name("Rover")
    end
  end

  def test_module_methods_can_affect_instance_variables_in_the_object
    fido = Dog.new
    assert_equal "Fido", fido.name
    fido.set_name("Rover")
    assert_equal "Rover", fido.name
  end

  def test_classes_can_override_module_methods
    fido = Dog.new
    assert_equal :in_object, fido.here
  end
end

# ------------------------------------------------------------------

# SCOPE

class AboutScope < Neo::Koan
  module Jims
    class Dog
      def identify
        :jims_dog
      end
    end
  end

  module Joes
    class Dog
      def identify
        :joes_dog
      end
    end
  end

  def test_dog_is_not_available_in_the_current_scope
    assert_raise(NameError) do
      Dog.new
    end
  end

  def test_you_can_reference_nested_classes_using_the_scope_operator
    fido = Jims::Dog.new
    rover = Joes::Dog.new
    assert_equal :jims_dog, fido.identify
    assert_equal :joes_dog, rover.identify

    assert_equal true, fido.class != rover.class
    assert_equal true, Jims::Dog != Joes::Dog
  end

	class String
	end

  def test_nested_string_is_not_the_same_as_the_system_string
    assert_equal false, String == "HI".class
  end

  def test_use_the_prefix_scope_operator_to_force_the_global_scope
    assert_equal true, ::String == "HI".class
  end

  MyString = ::String

  def test_class_names_are_just_constants
    assert_equal true, MyString == ::String
    assert_equal true, MyString == "HI".class
  end

############

  def test_you_can_define_methods_on_individual_objects
    fido = Dog.new
    def fido.wag
      :fidos_wag
    end
    assert_equal :fidos_wag, fido.wag
  end

  def test_other_objects_are_not_affected_by_these_singleton_methods
    fido = Dog.new
    rover = Dog.new
    def fido.wag
      :fidos_wag
    end

    assert_raise(NoMethodError) do
      rover.wag
    end
  end

############

  class Dog
    attr_accessor :name
  end

  def Dog.name
    @name
  end

  def test_classes_and_instances_do_not_share_instance_variables
    fido = Dog.new
    fido.name = "Fido"
    assert_equal "Fido", fido.name
    assert_equal nil, Dog.name
  end

  LastExpressionInClassStatement = class Dog
                                     21
                                   end

  def test_class_statements_return_the_value_of_their_last_expression
    assert_equal 21, LastExpressionInClassStatement
  end

# ------------------------------------------------------------------

# PROXY

class Proxy
  attr_reader :messages, :called

  def initialize(target_object)
    @object = target_object
    @messages = Hash.new(0)
  end

  def called?(method_name)
    @messages.has_key?(method_name)
  end

  def number_of_times_called(method_name)
    @messages[method_name.to_sym]
  end

  def messages
    @messages.keys
  end

  def method_missing(method_name, *args, &block)
    if @object.respond_to?(method_name.to_sym)
      @messages[method_name.to_sym] += 1
      @object.send(method_name.to_sym, *args, &block)
    else
      raise NoMethodError
    end
  end
end

# ------------------------------------------------------------------
# ------------------------------------------------------------------

# RAILS

# Asset Pipeline & Sprockets
# Asset pipeline is the structure, Sprockets is responsible for the actual requiring/loading

############

# CAPYBARA
# -mimics user behavior, and the language/methods are written to express that (visit_ , enter_ , click_ , page.shoud… , etc)
# -‘launchy’ gem helps debugging -> ‘save_and_open_page’ will open page at current state
# -capybara by default doesn’t support js, so we need to tell it support js and run w/selenium (some config necessary). then you can pass :js => true  option in the spec
# -GOTCHA => db records created ARE NOT AVAILABLE IN SELENIUM TESTS!
#  -this is because specs are using db TRANSACTIONS, which aren’t compatible w/selenium, so…
#  -in spec_helper file, set config.use_transactional_fixtures = false
#    -this will carry over db records between specs, which still isn’t good, so…
#      -use database_cleaner gem to clear out db between specs! (some config necessary)
#      -change strategy to truncation instead of transaction

# good for knowing you’ve redirected to correct place => current_path should eq(SOME_path)

# live reload gem - good to make auto changes w/o having to reload browser

############

# Sublime Text 2 settings
#  “trim_trailing_white_space_on_save”: true

# generate w/o assets
rails g controller SomeController —skip-assets
{% endhighlight %}

Whew. Lots of goodness in there. Next, my attempt at TDD with a simple Rails Todo app. There's one file in particular that I'd like to share here, and it's the TodosController spec:

**edit:** had to resort to screenshots because highlight was being a jerk and wouldn't let me change the tab spacing -___-

![todocontroller1][todocontroller1]

![todocontroller2][todocontroller2]

![todocontroller3][todocontroller3]

I've placed some comments in the file around areas that took me a while to figure out. I ultimately couldn't get the #destroy action to work how I wanted. I'm not sure why this is happening, but it says it can't find a Todo with some ID that's generated from the let!. I've looked for various spec expamples of this basic behavior, but didn't find anything helpful.

Here are some additional helpful screenshots:

![widget example][widget example]

![factory girl][factory girl]

![let explained][let explained]

![lazy eval][lazy eval]

![redirect][redirect]

![capybara][capybara]

Man, that tab spacing irritated me and now I'm just caffeinated and lazy :/

Welp, back to studying.

[todocontroller1]: 	http://i.imgur.com/nnwfdtJ.png
[todocontroller2]: 	http://i.imgur.com/30aL5jh.png
[todocontroller3]: 	http://i.imgur.com/7xmOwo4.png
[widget example]: 	http://i.imgur.com/zekKjOf.png
[factory girl]: 		http://i.imgur.com/XOrjaaR.png
[let explained]: 		http://i.imgur.com/iwcLiGb.png
[lazy evail]: 	 		http://i.imgur.com/kpxDtdm.png
[redirect]: 				http://i.imgur.com/y1pjyp2.png
[capybara]: 				http://i.imgur.com/ViUJnO4.png
