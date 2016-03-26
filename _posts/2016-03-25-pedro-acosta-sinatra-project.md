---
layout: post
title: "Sinatra project"
date: 2016-03-25
---


Hey guys, this is my second blog and today I'm going to talk about my sinatra project. If you read my first blog, you'll know that I'm currently a learn student in the Flatiron school. I went ahead and added my sinatra projects github page so you can have access to it.

This was my first sinatra project and I had a lot of fun doing it. Those of you new to programming, my word of advice is to not be afraid of challenging yourself. Embrace it. When you do, you'll learn a lot, have fun, and make an awesome project.

My sinatra project is pretty simple. I allow a user to make an account with the application. When the user makes an account he is able to create a league. What is a league? Well, for those of you who play sports, a league is just an organization where teams can compete against each other. So for every league you create, you can create teams for that league. For every team you create, you can create players, along with their jersey number and position they play.

If you take a look at my code, you can see that many of the controllers are very similar to each other and follow the same concepts and patterns. Getting each user to create leagues, teams, and players was pretty simple. To be honest, it was really easy. My struggles came down to saving each created player to their respected team. Same goes with the different teams to its league. I’m going to show you what I mean by that in the following code.

          [#<Player:0x007fb97c2c68d8
          id: 1,
          name: "Pedro Acosta",
          team_id: nil,
          league_id: nil,
          jersey_number: 6,
          position: "Center Field ">,
         #<Player:0x007fb97c2c6748
          id: 2,
          name: "Corry Harper",
          team_id: nil,
          league_id: nil,
          jersey_number: 34,
          position: "Outfield">,
         #<Player:0x007fb97c2c65e0
          id: 3,
          name: "Andrew C",
          team_id: nil,
          league_id: nil,
          jersey_number: 15,
          position: "Catcher">

As you can see, there are three player objects I created. Did you notice what is wrong with these objects? Well, what’s right about these player objects is that it stored the name, jersey number, and position of each players. What it doesn’t show is the players team id. The return is "nil". 

For those of you who haven't realize yet, I'm using the programming language Ruby. In ruby, there always needs to be a return value. When nothing is returned, you'll get "nil". It's pretty simple, nil is the Ruby object that represents nothingness. So ruby always has a return value.

The reason it returned nil is because nothing was saved into that object. Which means that my player object didn’t save its team it was assigned to. There lies the problem. I have players object and no way to verify who it belongs to. So what was happening was every team I created, when I was to click on their roster, it will show all of the saved player objects. It didn't matter what team it was. With the help of the learn developers, I was able to find a solution. 

          Players Controller:

        post '/players/:id' do 
          @player = Player.find_by_id(params[:id])
          @team = Team.find_by(params[:team_id])
          @player.team = Team.find_by(params[:team_id])
          @player.name = params[:name]
          @player.position = params[:position]
          @player.jersey_number = params[:jersey_number]
          @player.save 

          redirect "/players/#{@player.id}"
        end

        post '/players' do 
          @player = Player.new
          @player.team = Team.find_by_id(params[:team_id])
          @player.name = params[:name]
          @player.position = params[:position]
          @player.jersey_number = params[:jersey_number]
          @player.save
          #binding.pry
          redirect "/players/#{@player.id}"

        end

In my first post route I added two lines and the second post I added one.

      @team = Team.find_by(params[:team_id])
      @player.team = Team.find_by(params[:team_id])

The first line above is assigning @team instant variable to find the class team by its id. The second line is grabbing the first line and combining it with the @player instant variable. So the @player is looking for its team by its team id.
I then had to make some adjustments to my team show view and tested it out. 

       <ul>
        <% @team.players.each do |player| %>
          <li><%= player.name %>
          <%= player.position %> <%= player.jersey_number %></a></li>
        <% end%>
      </ul>

Here I iterated each player through its team. It then will display each players name, position, and jersey number. It worked! Great news was I was able to mimic this simple pattern on to my teams and league controllers. 

    Teams controller:

    post '/teams/:id' do 
      @team = Team.find_by_id(params[:id])
      @league = League.find_by_id(params[:league_id])
      @team.league = League.find_by_id(params[:league_id])
      @team.name = params[:name]
      @team.save

      redirect "/teams/#{@team.id}"
    end

    post '/teams' do 
      @team = Team.new
      @team.league = League.find_by_id(params[:league_id])
      @team.name = params[:name]
      @team.save

      redirect "/teams/#{@team.id}"
    end

    Leagues controller:

      post '/leagues' do 
        @league = League.new
        @league.name = params[:name]
        @league.save

        redirect "/leagues/#{@league.id}"
      end

      post '/teams' do 
        @team = Team.new
        @team.league = League.find_by_id(params[:league_id])
        @team.name = params[:name]
        @team.save

        redirect "/teams/#{@team.id}"
      end

I also tested it out in my leagues show view by applying the same logic.

      Leagues show view:

        <ul>
          <% @league.teams.each do |team| %>
            <li><a href="/teams/<%= team.id %>"><%= team.name %></a></li>
          <% end%>
        </ul>

This project was really fun. Especially when I was debugging my player objects and figuring out why it didn’t want to save its team id. It took me a week just to figure that out so that wasn't really fun. I learned a lot and took risks of my code breaking. I hope someone can make this application useful for themselves.

If you want to interact with my application, here's the github page and clone this project.

https://github.com/pacosta613/pacosta613.github.io.git

Have Fun! Learn, Love, Code.