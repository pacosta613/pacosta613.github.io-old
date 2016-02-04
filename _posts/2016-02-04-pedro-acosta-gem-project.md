---
layout: post
title: "CLI gem project"
date: 2016-02-04
---

I finally was able to complete Learns CLI gem project and submitted it online. For those of you who don't know, in order to compelete this project, you needed to submit a video discussing your code and show some of the process of you coding and submit a blog discussing your code.

This is my first time doing this so I might struggle putting words together. Im going to discuss a couple of methods in this project.

The first part of the project was in the vegas-cli.rb file. The second was vegas-sports.rb file. Both under the lib directory and vegas_odds folder. Ill add my github link to this project at the end of this blog just so users can interact with it. Ill start to talk about the football method which is in the vegas-cli.rb file.

      def football
        football = VegasOdds::Sports.now[0]
        puts "#{football.title} will be hosted on #{football.date} on Fox sports."
        puts "#{football.away} vs. #{football.home}."
        puts "Do you want to see the odds of this game?:"
        input = gets.chomp
        if input == "yes"
          puts "#{football.away} #{football.odd} vs. #{football.home} #{football.odd_2}"
          puts "Do you want to bet on this game?"
          input = gets.chomp
          if input == "yes"
            bet
          else input == "no"
            football
          end
        else input == "no"
          opening_statement
        end
      end

Off the bat it may seem confusing but it really isn't. Ill explain to you what I did.

I assigned a local variable to the VegasOdds::Sports class and pushed the #now method with it. If you notice after the now method, there is a bracket with 0; [0]. In the now method, which is located in vegas-sports.rb file, contains three other methods with certain criteria of information. Basically the [0] is calling the first method which is the self.scrape_football method.

Now that its being called, I have the ability to display certain information from the scrape_football method. All that was left was figuring out what I wanted to be display for the user and how the user was going to interact with it.

I decided to use puts and variables that were the source of the information I needed. For ex, the football(is the local variable) and the .title method displays the title of the event, which is Superbowl 50. 

            input = gets.chomp

What is being done here is, the app is going to take the input the user enters in and depending on the answer of the user, the app will either display a certain information or pushes the user to start from the beginning of the app.

    if input == "yes"
      puts "#{football.away} #{football.odd} vs. #{football.home} #{football.odd_2}"
      puts "Do you want to bet on this game?"
      input = gets.chomp
      if input == "yes"
        bet
      else input == "no"
        football
      end
    else input == "no"
      opening_statement
    end

The if, elsif, and else statement is what allowed the app to display certain information depending on the input the user gave it. So in the scenario the user said yes, it will display the information it asked for and if the user said no, it will restart the app. 

      puts "Do you want to bet on this game?"
      input = gets.chomp
      if input == "yes"
        bet
      else input == "no"
        football
      end

Going deeper in this method. Inside the first if statement included another if statment. Essentially recreating more options for the user. When the user saw the odds that were being displayed for the game, it was ask another question: 

      puts "Do you want to bet on this game?"

Depending on the answer the user gave, if input was yes it would call the bet method, which basically allowed the user to bet on the game or person and if input was no, it would force the user to start from the beginning of this method; or in other worlds this particular sport.

I used the same concept with the other methods throughout this file. Even though it didnt seem too abstract for me, it did what I originally intended the project to do. I added too much puts but I didnt want the user to fill limited as to what he can do with this app.

This project was pretty fun. Took me a couple of days to complete due to certain codes which was not responding the way I wanted it to, but I learned a lot. 

Here is the URL for this project https://github.com/pacosta613/vegas_odds.git.

If you wanted to try to interact with the gem yourself just follow the steps below:

Add this line to your application's Gemfile:

gem 'vegas_odds'

And then execute:

$ bundle

Or install it yourself as:

$ gem install vegas_odds

P.S. I saw a couple of mistakes on my projects so I'm going to push the update to my github page.

