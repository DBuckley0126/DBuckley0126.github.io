---
layout: post
title:      "Rails Project — Wonky"
date:       2020-01-31 08:12:09 -0500
permalink:  rails_project_wonky
---


The Medium Article: https://medium.com/@dannyboy.msn/rails-project-wonky-bd468aa27dcb

A Flatiron Rails project —A online marketplace for farmers to list produce that is rejected by supermarkets.

For my third Flatiron project, the project requirements were:
* Use Ruby on Rails as the framework
* Use complex active record associations
* Defend against valid data including validation error display
* User authentication including 3rd party authorisation
* Nested resources

**Defining the Problem**

Consumers throw away almost as much food as they eat because of a “cult of perfection”, deepening hunger and poverty, and inflicting a heavy toll on the environment. Vast quantities of fresh produce grown in the US are left in the field to rot, fed to livestock or hauled directly from the field to landfill, because of unrealistic and unyielding cosmetic standards. This food is perfectly good to eat and should be eaten.

People have become reliant on the supermarket as their go-to solution for food. Living in a world where all food produced is to these blemish-free and perfectly shaped. Pushing the industry to even go to the lengths of genetically modifying fruit and vegetables to create the ‘perfect’ produce.

There needs to be education and information provided about what is actually being wasted between the moment the seed is put in the ground, till the moment its on the end of your fork. People need to become aware there are other ways to access fresher, cheaper and more natural produce outside of the traditional supermarket.

**The Solution**

Wonky is an online marketplace that farmers can register their farm, and post listings of produce, including a description of how it was grown, if it was grown organically and the selling price of the product per selected unit of measurement. Purposely avoiding image uploading to listings to push forward the idea to not judge fruit and vegetables by appearance. Users can then view all the listings on the homepage and filter results by category and distance. Users can make an account and create orders on the listings which notifies farmers of customer pickup.

This marketplace will also provide information about the food chain, highlighting issues within the food industry and providing stats on all the products being brought through the Wonky including an active tally of the CO2 and food miles being saved by buying locally.

**The Inner Workings**

The project consists of 5 models, User, Farmer, Listing, Purchase and Category. These all connect to a PostgreSQL database formed of a detailed schema outlining over 40 columns for all the models. Nested resourcing was implemented to achieve RESTful patterns across the whole application. Logged in as a Farmer, you can create listings through a farmer path ‘/farmer/:farmer_id/listings/new’ for example. Logged in as a user you could view a purchase from a user path ‘/user/:user_id/purchase/:id’.

I understood that location would play an important part in the experience. Having a visual understanding of the locality of the products a user is looking at, gives a greater appreciation of the distance food is sourced from. Geocoder is a Ruby gem which provides a competent solution to connecting to location API services. Incorporating Geocoder to User and Farmer models provided a bunch of useful instance methods. For example, find the distance between two objects or find all the object instances within a given distance, or a whole range of other capabilities. I added latitude and longitude column to User and Farmer models and set up Geocoder to update the coordinates of an instance before validation. I also added custom validation to the models preventing instances being saved with invalid addresses, avoiding issues coming from inaccurate location data. Users and Farmers can now have their distance calculated between them, allowing for distance filters to be added to listing results.

Using this data, I incorporated a static Google map API, fetching an image of a given map area, indicated by coordinates in a post request. I set up methods to dynamically create URLs containing farmer coordinates of a given array of listings. Also setting up the URL to overlay numbered markers at farm locations reflecting the numbers on the listing results. The map is updated after each search/filter result, with the markers reflecting the results in realtime. The map image can also be clicked on to take the user to Google maps automatically opening up directions.

Incorporating Google authentication was a little tricky. The way the controllers was set up was not standard as there were two controllers handling signup (user & farmer) and one session controller handling login. Once a user successfully authenticates through Google, deciding what path to redirect to, keeping separation of concerns in place, was a struggle. I ultimately decided to create a new controller to handle 3rd party authentication. Once a user successfully authenticates, a post request goes to the new controller. Using ActiveRecord to query the database for any users or farmers which match the UID provided from the request. If a match is found, the user_id is added to the session and redirected to the root path. If no records are returned, it is deemed this is a signup request and therefor rendering a form to allow for the rest of the profile to be completed to allow fulfilling validation requirements. Once this is submitted successfully, the user is logged in.

**If I could reverse time**

Starting a project is always overwhelming. A whole day of google searching trying to prepare for the project can leave you even more overwhelmed by the huge scope of options and opinions available on the internet. One popular option I tried implementing was using Device for all user authentication handling. The huge capability of Device comes with is a huge range of configuration options. Trying to set up routes for the two user configuration I wanted, was causing more trouble than expected. So I resorted to creating my own custom authentication which allowed much greater power over the data and views. The lesson learnt was there is always a pre-made solution, but sometimes, doesn't mean its always the best solution.

