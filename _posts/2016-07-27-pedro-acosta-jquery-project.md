---
layout: default
title: "Jquery project"
date: 2016-07-27
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

Once this event is triggered, we can starting getting the information needed using a AJAX request. Once the AJAX request is done, we append the the response to the div class ".ingredients". This completes the form submission and now we'll go over a bit about the server side of the response.

The form that is being submitted is located in the recipes show page. That form was to add ingredients. Now that form is functional, I decided to render the new ingredients to the recipes show page without the page reloading. I wrote two functions to make this render correctly but will explain what I did one at a time. 

```ruby

function loadIngredients(){
  $('#recipe_ingredients').html('<h3>Ingredients:</h3>')

  $.ajax({
    method: 'GET',
    dataType: 'json',
    url: this.action
  }).done(function(response){
    grabIngredients(response);
  });
};

```

My loadIngredients function starts off with some JQuery which renders "Ingredients:" before the actual lists. I then make a AJAX request to grab the response. I used a get request to send data to my rails backend and this.action just refers to the current URL. When it recieves the response in JSON, I called grabIngredients function to parse the response.

```ruby

function grabIngredients(data){ 
  var ingredients = data["ingredients"];
  var names = [];
  var nameId = [];
  var orderIngredients = "<ol>";
  
  for (var i = 0; i < ingredients.length; i++) {
    names.push(ingredients[i]["name"]);
    nameId.push(ingredients[i]["id"]);
  }
  for (var i = 0; i < nameId.length; i++) {
    orderIngredients += "<li>" + names[i] + "</li>";
    
  }
  orderIngredients += "</ol>";
 $(".ingredients").html(orderIngredients);
}

```

Now that we have the response from our AJAX request, we can now parse the data. We will parse that data by looping through each ingredient, pushing the name and its id to an empty array. We then add another loop whichs goes through each ingredients id in nameId array, which renders the name of ingredients in a list. Finally we append each ingredient to the div class ".ingredients" in our recipes show page.

We now have the ability to add ingredients to our recipe show page without having the page reload. It renders perfectly and pretty fast.

I'm still not really confident in my ability to code but I'm learning a lot and continue to improve. I hope you guys learned something from this blog!

If you want to interact with my application, here's the github page repo.

https://github.com/pacosta613/recipe-project-jquery

Have Fun! Learn, Love, Code.