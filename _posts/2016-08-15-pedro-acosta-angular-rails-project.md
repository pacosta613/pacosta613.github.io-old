---
layout: post
title: "Angular/Rails project"
date: 2016-08-15
---

Hey guys! I finally was able to finish my final project for [Learn](https://learn.co). It was pretty exciting project that potentially can be use on a daily basis. For those of you who don't know, I currently play baseball in a mens league called MABL. I purposely made this app so that one day(hopefully next season) we can use this app for scheduling games, posting the outcome of those games, statistics, and more. It was a lot of fun creating this app and Im thrilled of the outcome. I will go through some important code for this app. 

I will start of giving a breif discussion of the backend of the app, which in this case was Rails. The app is based on a league having multiple divisions, with teams in those divisions, and players in those teams. So our models relationship will be like so,

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

So as you can see, our players have some properties such as first name and last, our division and teams both have name but since we know team belongs to a division it has a           key. Now that this is set up, we create controllers for divisions, team, and players, and now we have a functioning backend of the project completed. 

Now lets check out angular, which is the front-end side of the app.

Angular in a way, as some similar characteristics with rails. Both have controllers, views, and models that help construct 