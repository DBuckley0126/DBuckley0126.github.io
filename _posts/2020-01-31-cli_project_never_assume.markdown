---
layout: post
title:      "CLI Project — Never Assume"
date:       2020-01-31 13:04:22 +0000
permalink:  cli_project_never_assume
---


So I had been racking my head for ideas while working my night shift the week before the project week, as I knew if I was to make the best dawn command-line interface program any terminal has ever seen, I would need to be passionate about my project. While I was stacking cans of tomatoes, the idea came to me… Build a CLI game which the user is presented with a phrase and has to choose out of a range of options which google autocomplete search result, comes up first when the phrase is entered into the google search bar. I quickly brainstorm how I would make this project meet the requirements:
* Provide a CLI
* Your CLI application must provide access to data from a web page.
* The data provided must go at least one level deep. A “level” is where a user can make a choice and then get detailed information about their choice.

I quickly searched for websites which I can scrape the data from, and came across a basic game which is already completing my desired logic. I inspect the page to see how the data was being presented. It appeared all the data was being stored as text within divs. Perfect! I quickly make a test program, require Nokogiri and open-URI module, import the HTML from the website, started narrowing down the CSS nodes, got to the div which should have contained the text I so desperately wanted. But there was no text, there were no values, I could not understand what was going on. After much frantic google searching, it appears I have hit a wall. The HTML elements were actually being filled with the text I wanted from some initial javascript that was being started every time the page loads. If this was a Ruby on Rails Project, you could do some complex code to receive the page after the javascript has run, but this was not possible with the week time schedule.

So I had to radically change my idea, still within the idea of some type of game which used data scraped from a website for the user to interact with. Listening to Spotify at the time, I always wondered how some of these terrible songs were in the charts. My second idea! I knew there are many websites which contained chart information history from over the years. How about a game where the user has to assume if a song is higher or lower in the chart than another song? I would allow the user to input a year, in which the chart data would be scrapped for that year, and then manipulated in such a way that the data can be presented to the user. Then the user could interact with the data, giving higher or lower answers. Many classes, objects, gems, css selectors, methods, modules later. I have completed the game. Assumption. It works really well, and actually happy that the first idea didn’t work out after all. I have learnt a lot about scraping data from websites, one of the things never assume a websites HTML is always as it seems.
