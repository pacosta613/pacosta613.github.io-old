---
layout: post
title: "Rails project"
date: 2016-06-24
---

Hey guys! This is going to be a short blog but I will try to be as descriptive as possible. It took about less than a week for me to complete this project. I pretty much had to implement some jquery and ajax to my previous rails project. It was a lot of fun and I learned a lot. Lets get to it. 

If you don't already know, this was a recipe app. It pretty much allows users to create a recipes, along with it's ingredients. It also allows users to comment on each recipe and rate each recipe. So, there are five requirements to complete this project,

```ruby 

1. Must render one show page and one index page via jQuery and an Active Model Serialization JSON Backend.
2. Must use your Rails api to create a resource and render the response without a page refresh.
3. The rails API must reveal at least one has-many relationship in the JSON that is then rendered to the page.
4. Must have at least one link that loads, or updates a resource without reloading the page.
5. Must translate the JSON responses into Javascript Model Objects. The Model Objects must have at least one method on the prototype. Formatters work really well for this.

```

I'm going to explain my some thought process and what I did to fulfill requiement number two. Essentially, I'll be overriding the default behavior of a form submission and using AJAX post requests, configuring server responses, and deciding how its being handle.

The first thing we need to do is to tackle the default behavior of the form submission. When a user clicks on a forms submit button, it sends a POST request with its url, the server handles the response, and then the browser redirects to a new route. We want to prevent the browsering redirecting to a new route. So what can we do? Thanks to JQuery we can attach an event listerner to the form to prevent this entire process when the forms submit button is clicked, like so


```ruby

function newIngredients(){
  $("#new_ingredient").on("submit", function(e){
    e.preventDefault();

    $.ajax({
      method: 'POST',
      url: this.action,
      data: $(this).serialize(),
      dataType: 'json'
    }).done(function(response){
      $(".ingredients ol").append("<li>" + response["name"] + "</li>");
      $("#ingredient_name").val("");
    });
  });
};

```

You can see in the newIngredients function I used a JQuery selector "#new_ingredient", which is the forms id. The following line is what prevents the entire form from redirecting to a new route.

```ruby

e.preventDefault();

```

Once this event is triggered, we can starting getting the information needed using a AJAX request. 