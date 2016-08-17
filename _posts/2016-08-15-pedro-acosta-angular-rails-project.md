---
layout: post
title: "Angular/Rails project"
date: 2016-08-15
---

Hey guys! I finally was able to finish my final project for [Learn](https://learn.co). It was a pretty exciting project that can potentially be used on a daily basis. For those of you who don't know, I currently play baseball in a mens league called MABL. I purposely made this app so that one day (hopefully next season) we can use this app for scheduling games, posting the outcome of those games, statistics, and more. It was a lot of fun creating this app and Im thrilled wih the outcome. I will go through some important code for this app. 

I will start by giving a breif discussion of the backend of the app, which in this case was Rails. The app is based on a league having multiple divisions, with teams in those divisions, and players in those teams. So our models relationship will be like so,

```ruby

  class Division < ActiveRecord::Base
    has_many :teams
  end

  class Player < ActiveRecord::Base
    belongs_to :team
  end

  class Team < ActiveRecord::Base
    has_many :players
    belongs_to :division
  end

```

and our tables, along with its properties will look like this,

```ruby

class CreatePlayers < ActiveRecord::Migration
  def change
    create_table :players do |t|
      t.string :first_name
      t.string :last_name
      t.string :position
      t.integer :team_id
      t.integer :division_id
      t.integer :jersey_number

      t.timestamps null: false
    end
  end
end


class CreateDivisions < ActiveRecord::Migration
  def change
    create_table :divisions do |t|
      t.string :name

      t.timestamps null: false
    end
  end
end

class CreateTeams < ActiveRecord::Migration
  def change
    create_table :teams do |t|
      t.string :name
      t.integer :division_id

      t.timestamps null: false
    end
  end
end


```

So, as you can see, our players have some properties such as first name and last, our division and teams both have name but since we know team belongs to a division it has a foreign key. Now that this is set up, we create controllers for divisions, team, and players, and now we have a functioning backend of the project completed. 

Now lets check out angular, which is the front-end side of the app.

Angular in a way, has some similar characteristics with rails. Both have controllers, views, and models that help construct the layout of the app. My opinion of what makes them different is that rails makes the application work the way it does, angular just makes it look much prettier and adds little cool features. 

Going to go over two things on the angular side, the app module and one of the controllers. This is the app module,

```ruby

angular
 .module("app", ["ui.router", "ngResource", "templates"])
 .config(function($stateProvider, $urlRouterProvider) {
   $stateProvider
     .state("league", {
       url: "/",
       templateUrl: "home.html",
       controller: "DivisionsController as ctrl"
     })
     .state("league.new", {
       url: "new",
       templateUrl: "division/new.html",
       controller: 'DivisionsController as ctrl'
     })
     .state("league.divisions", {
       url: "divisions",
       templateUrl: "division/divisions.html",
       controller: "DivisionsController as ctrl"
     })
     .state("league.division", {
        url: 'division/:id',
        templateUrl: 'division/show.html',
        controller: "DivisionsController as ctrl"
     })
     .state("league.edit", {
        url: 'edit/:id',
        templateUrl: 'division/edit.html',
        controller: 'DivisionsController as ctrl'
     })
     .state('league.teams', {
        url: "teams",
        templateUrl: 'team/teams.html',
        controller: 'TeamsController as ctrl'
     })
     .state('league.new-team', {
      url: 'new-team',
      templateUrl: 'team/new.html',
      controller: 'TeamsController as ctrl'
     })
     .state('league.edit-team', {
      url: 'edit-team/:id',
      templateUrl: 'team/edit.html',
      controller: 'TeamsController as ctrl'
     })
     .state('league.team', {
      url: 'team/:id',
      templateUrl: 'team/show.html',
      controller: 'TeamsController as ctrl'
     })
     .state('league.players', {
      url: "players",
      templateUrl: 'player/players.html',
      controller: 'PlayersController as ctrl'
     })
     .state('league.player', {
      url: 'player/:id',
      templateUrl: 'player/show.html',
      controller: 'PlayersController as ctrl'
     })
     .state('league.new-player', {
      url: 'new-player',
      templateUrl: 'player/new.html',
      controller: 'PlayersController as ctrl'
     })
     .state('league.edit-player', {
      url: 'edit-player/:id',
      templateUrl: 'player/edit.html',
      controller: 'PlayersController as ctrl'
     })
     .state('league.about', {
      url: 'about',
      templateUrl: 'about/about.html'
     });
  $urlRouterProvider.otherwise("/");
});

```

As you can see, there seems to be a bit of repetitive code but the most important thing here is where you see ".state". State in other words, pretty much allows us to call routes. States take in two arguments, our routes name and an object with keys each state will use such as the url, templateUrl, and controller. You can see some nested routes in some of the states. Some states have certain controllers with code that take care of certain tasks and the template/view for that state.

The Division Controller

```ruby

function DivisionsController(Division, $location, $state, $stateParams, Team) {
  var ctrl = this; 
  ctrl.divisions = Division.query();
  ctrl.newDivision = new Division();
  ctrl.team = new Team();
  ctrl.division = Division.get({id: $stateParams.id});

  ctrl.addDivision = function() {
    ctrl.newDivision.$save(function() {
      $location.path('divisions');
    });
    $state.go($state.current, {}, {reload:true});
  };

  ctrl.deleteDivision = function(division){
    division.$delete(function(){
      $state.go($state.current, {}, {reload:true});
    });
  };

  ctrl.editDivision = function(){
    ctrl.division.$update(function(){
      $location.path('divisions');
    });
  };

  ctrl.addTeam = function(team, division){
    team.division_id = division.id;
    
    team.$save(function(result){
      console.log(result);
    });
    ctrl.team = new Team();
    $state.go($state.current, {}, {reload: true});
  };

};

angular 
  .module('app') 
  .controller('DivisionsController', DivisionsController);

```

This is the divisions controller and it has 4 functions in it. The four functions allows the user to create, delete, and edit a division. The final function allows the user to create a team with that division. At the top you see some lines of code and I will get to what they do.

```ruby

var ctrl = this; 
  ctrl.newDivision = new Division();

```

The first like im creating a variable called "ctrl" and asssigning it to "this", which means the current path its in. In this case its the controller. 

```ruby

ctrl.divisions = Division.query();

```

All this line is doing is query/getting every single division object. It pretty much allows the user to show all the divisions in the database. Simple line.

```ruby

ctrl.team = new Team();
ctrl.division = Division.get({id: $stateParams.id});

```

The first line is pretty self explanatory. It's just a variable which allows you to create a new team object. The second line grabs the currents divisions id using $stateParams service angular provides for us. 

I tried my best to be a bit descriptive about some of my code. Again, I'm slowly getting the hang of this, but still have a lot more room to grow. I hope you guys enjoyed this explanation and look forward to creating a new post soon.

If you want to interact with my application, here's the github page repo.

https://github.com/pacosta613/Baseball_Factory.git

Have Fun! Learn, Love, Code.