---
layout: post
title: "Rails project"
date: 2016-06-24
---

Hey guys! After a long battle and countless hours of work, I was finally able to complete my rails project. It took me sometime to do. I even had to re-create it just to gain a better perspective on rails. Rails is difficult at first because of all its "magic", but once you get a deeper understanding of what's going on in the backend it'll click right away. 

This is a pretty simple rails app. It's a recipe app. The goal was just to allow users to create a recipe along with all of its ingredients. It also allows users to comment and rate on their own or other users recipes. There are some additional features that I will add later on to this app such as giving certain users roles using pundit, which is pretty much allowing and limiting users to deleting other users recipe, comments, ratings etc. 

I'm going to explain my thought process with some of my code and try to explain it in the best techincal way I can. Again, I still don't consider myself good at programming yet, but I just want to keep working on getting better as best I can. 

Using Devise and Omniauth

When you start the application you're sent to the home page. In order to start creating and seeing other users recipes you will need to register/login into an account. I used two authentications in this app, devise and omniauth.

The devise gem comes with a lot of neat features and it's highly customizable. It takes care of so many different aspects for you such as controllers, views, mailers, errors, and routes. This is pretty much what a user will need to signin/register themselves with the app. Its a pretty popular gem to use and its pretty dependable. 

The Omniauth gem is pretty cool too. Let me explain what it is. Ever go to a website and decide to register with them? When you start the registration it gives you an option to signin with google, twitter, Facebook, etc. Well, thats pretty much the omniauth gem. There are so many omniauth gems available to you but I choose to use facebook. Now users have the options of creating an account with their email or facebook account.

Structure

Now the most difficult part of this project is deciding the structure of your app. There are already guidelines and requirements Learn.co requires inorder for you pass this project. Now that we have the user models, controllers, and views set up, we can start creating how I want the app to function. 

The user should be allowed to create a recipe. The recipe has many ingredients, comments, and rating. FYI I used a gem for the ratings, just wanted to add some pretty jQuery for the ratings. Now even though recipe has many ingredients, I also want the ingredient to have many recipes. For this task, I'll need a ActiveRecord join table. The join table will be called recipe_ingredients. 

Recipe model looks like this:

```ruby
class Recipe < ActiveRecord::Base

  belongs_to :user
  has_many :comments
  has_many :recipe_ingredients
  has_many :ingredients, through: :recipe_ingredients

  validates :name, :presence => true
  validates :name, :uniqueness => true

end

```

Ingredient model looks like this:

```ruby

class Ingredient < ActiveRecord::Base
  has_many :recipe_ingredients
  has_many :recipes, through: :recipe_ingredients
  validates :name, :presence => true
  validates_uniqueness_of :name
end

```

I went ahead and added some validations to both models just to avoid any dublicate names and make sure the user doesn't leave any blank spaces in the forms.

This is what got me started and allowed me to complete this project. It's a simple app. Feel free to use it. I will soon add some jQuery to the entire app for my next project. I had a lot of headaces and fun building this app and look forward to improving my programming skills. 

If you want to interact with my application, here's the github page repo.

https://github.com/pacosta613/recipes-project-2.0.git

Have Fun! Learn, Love, Code.