---
layout: post
title:      "Sinatra Project — Remember Me"
date:       2020-01-31 08:09:36 -0500
permalink:  sinatra_project_remember_me
---

The Medium article: https://medium.com/@dannyboy.msn/sinatra-project-remember-me-d71bc9a13c1d

A Flatiron Sinatra project — An Alexa assistant to help you to remember the little things in life with a simple phrase and answer.

For my second Flatiron project, the requirements it had to meet were:

* Be a MVC Sinatra web application
* Use active records
* Implement user accounts
* Use CRUD

**Defining the Problem**

Both in life and at work, we tend to come up with solutions before defining the problem they solve. Coming up with a solution is a rewarding experience, but is it really a solution when there is no problem?

Problems, on the other hand, are much harder to articulate. We don’t want to think about our problems or needs. They’re a pain.

I wanted to create a web application which offered a solution to an existing problem. So I went about my daily life waiting for a problem to arise, but during a busy day, you come across many problems, and for a forgetful person like me, you forget most of them by the end of the day. Hang on, there's a problem. Peoples lives are very busy and some people just don't have time to get a notebook out or unlock a phone to write something down to remember for later. This needs a solution.

**Creating the Solution**

I struggle to remember a lot of small things throughout the day, and wouldn't it be perfect if an assistant could just remember all this for me, in a quick and convenient way? This is where my application Remember Me comes to upgrade your frontal cortex.

Voice assistant adoption is at a record high, with over half the U.S. population regular users of a voice assistant, it is clear that this is a new convenient way of accessing data, offering an alternative to a normal user interface. Being able to have a natural conversation with an application is very convenient, and would make a perfect companion for the application.

So I decided that I would make an Alexa skill which a user can have a conversation with to ask it to remember absolutely anything. I would also have a web application to manage the user account to allow for the viewing/editing of everything Remember Me is actively remembering

I started with the base Sinatra application web interface, creating views for user signup, login, homepage which displays users Remembers. Then with the use of ActiveRecord and SQLite, created routes which handled user creation and validation, and saving them into a local database. The Input validation that was implemented checked email uniqueness and validity. User passwords within the sqlite3 database were encrypted with the bcrypt ruby gem which provided a layer of security. Error notifications were added to every route using a gem called Sinatra Flash, which added custom error data to a users session within a POST route before being redirected. I then added the ability to add Remembers to a users account from within the web application, with a few more views and routes handling the data.

The application worked as intended, users could create and read there own Remembers. But now it was time to make the application fulfil its dreams, of being the smartest assistant for remembering things to ever grace the world wide web.

**Alexa Integration**

Previous to this I had not used any APIs in my coding, i actually have not even used Alexa in my entire life. So immediately, I knew I was in for a huge learning curve. Alexa Developer platform was refreshingly welcome for anyone to develop on it and use their highly intelligent tools to create Alexa skills for the public. Looking through many stack overflow posts, and studying the Alexa developer centre, I got to work creating the voice of Remember Me.

First, I created an Invocation for the skill. Users say a skill’s invocation name to begin an interaction with a particular custom skill.

> Alexa, launch remember me

Then created an Intent for the skill. This is an action within the skill that can be triggered when a user says a particular utterance. You provide a range of sample utterances which you can allocate slots into. These slots have a set datatype. This can be city’s, numbers, dates. I chose to train a custom slot datatype to increase the accuracy of user commands. To train a slot type, you give it lots of sample data of the type of things which would normally fit in the slot. Then a clever machine learning model is used to assist matching user input to the custom slot type. The custom slot performs better over time as it learns from user input.

> Alexa, ask remember me to remember when i need to pick the kids up with the answer 9pm

On confirmation of user input, the data is sent in a JSON post request to an endpoint set by me. I set up a post route within my application and handled the request with a gem called Ralyxa, which converted the JSON request into an object which could have methods called upon it to interact with the data. The requests trigger specific intents set within the application. Within these intents, the data is taken from the JSON file and then a range of ruby helper methods deal with Alexa account creation, getting user data securely from Amazon API, inserting phrase and answer into the database, and then sending a response back to Alexa API to feedback to the user. This all happens within a few milliseconds.

After I confirmed this worked by using the Alexa developer simulator (it took me about 100 attempts to confirm it worked as intended). I created a other intent to handle a request from the user asking for a previous Remember.

> Alexa, ask remember me what it knows about when I need to pick the kids up

After much much debugging and screaming at my computer, learning about fuzzy-matching algorithms, implementing AES256 data encryption, and sending temporary password emails through a mail jet API. I can happily say Alexa has full interaction with the Remember Me database!

**Conclusion**

The huge learning curve during this has been very tough at times, especially doing this within the time limit, I've had to work very late into the night, every single night, to make this application come to life. I am thrilled to say that the Alexa skill has passed the Amazon verification process and the Remember Me application is now being hosted on Heroku, with user and Alexa accounts being fully integrated into the service. I will continue to develop the application and proudly present it within my portfolio of projects.



