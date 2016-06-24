---
layout: post
title: "Rails project"
date: 2016-06-24
---

Hey guys! After a long battle and countless hours of work, I was finally able to complete my rails project. Took sometime to do. I even had to re-create it just to gain a better perspective on rails. Rails is difficult at first because of all its "magic" but once you get a deeper understanding of whats going on in the backend it'll click right away. 

This is a pretty simple rails app. It's a recipe app. The goal was just to allow users to create a recipe along with all of its ingredients. Also allows users to comment and rate on their own or other users recipes. There are some additional features that I will add later on to this app such as giving certain users roles using pundit, which is pretty much allowing and limiting users to deleting other users recipe, comments, ratings etc. 

Going to explain my thought process with some of my code and try to explain it in the best techincal way I can. Again, I still don't think im good at programming yet but I just want to keep working on getting better as much as I can. 

Using Devise and Omniauth

When you start the application you're sent to the home page. In order to start creating and seeing other users recipes you will need to register/login into an account. I used two authentications in this app, devise and omniauth.

The devise gem comes with a lot of neat features and it's highly customizable. It takes care of so many different aspects for you such as controllers, views, mailers, errors, and routes. This is pretty much what a user will need to signin/register themselves with the app. Its a pretty popular gem to use and its pretty dependable. 

The Omniauth gem is pretty cool too. Let me explain what it is. Ever go to a website and decide to register with them? When you start the registration it gives you an option to signin with google, twitter, Facebook, etc. Well, thats pretty much the omniauth gem. There are so many omniauth gems available to you but I choose to use facebook. Now users have the options of creating an account with their email or facebook account.

Structure

Now the most difficult part of this project is deciding the structure of your app. There are already guidelines and requirements Learn.co requires inorder for you pass this project. Now that we have the user models, controllers, and views set up, we can start creating how I want the app to function. 