---
layout: post
title:      "How to build a physics Javascript game"
date:       2020-01-31 13:19:34 +0000
permalink:  how_to_build_a_physics_javascript_game
---


The Medium article: https://medium.com/@dannyboy.msn/how-to-build-a-physics-javascript-game-bcbb3a59288a

A Flatiron Javascript project — Gryo is a javascript game which runs simultaneously on your phone and desktop, using the mobile device gyroscope sensors as the desktop game input for control of a spaceship in a zero-gravity environment.

**Introduction**

For my javascript project at Flatiron, I wanted to create something that pushes the limits of my knowledge and going to test my capability to learn new tools and libraries in a limited amount of time. I can safely say this has been one of the most challenging things I’ve set myself. I threw myself into the completely unknown to create something that I had no idea how to. What made the process extra challenging is that there were limited examples of similar projects on the internet to help conjure up my idea.

So I am going to write this, a short overview of the tools and processes I used to create this game. So if any future student who wants to spend many many hours hitting their head against a desk to code a similar project. Then you can read this and hopefully have to hit your head a few fewer times to get the end result!

**The project**

The main gameplay elements I wanted to include:
* Have a mobile device act as a controller for a simultaneously running application running on a desktop computer.
* Use the gyroscope sensors on the mobile device as a gameplay mechanic
* Incorporate real-time physics

I decided on an arcade-style vertically scrolling shooter with asteroids flying towards you which you have to break up with your plasma cannon. The spaceship (player) is controlled with the mobile device gyroscope sensors. The game gets progressively harder with more asteroids and speed being added over time. Once the player gets hit by an asteroid, game over!

The final outcome is pretty decent if I do say so myself. Why don’t you have a look for yourself!

https://javascript-project-gyro-front.herokuapp.com/

*Please report any of the many bugs to me.*

**Networking: WebSockets (ActionCable)**

To create the connection between the mobile device and the desktop, HTTP requests were not going to cut it any longer. The project needed near-instant communication with data being transferred many times a second.

This is where the technology WebSockets comes to save the day. It removes the unnecessary overhead that HTTP requests use when transferring data, causing latency between the server and the user.

WebSockets provide a persistent connection between a client and server that both parties can use to start sending data at any time.

The client establishes a WebSocket connection through a process known as the WebSocket handshake. This process starts with the client sending a regular HTTP request to the server. An upgrade header is included in this request that informs the server that the client wishes to establish a WebSocket connection.

WebSockets from my understanding, seem very complex to incorporate with your solution, so Rails came along to also, save the day, with their solution ActionCable. It seamlessly integrates WebSockets with the rest of your Rails application using similar Ruby code that you’ve already learnt!

A very simple overview of how it works:

Within the Rails application, you will now have a folder called channels, which unsurprisingly hold your channels you will be writing. Think of these channels in a very similar way to your rails controllers. They act as a middleman between your Rails models and your frontend code. Each channel holds methods which can be called upon by front-end Javascript to execute any backend Ruby code you would like.

These channels can be subscribed to by any amount of users(consumers) to create a real-time tunnel of information between the backend and the user. These channels can create private instances of themselves which only certain other users can join or they can be public instances that any users can subscribe to. All channels are identified by a unique identifier, which is how initial subscription requests are matched up. These unique identifiers could be any data type, even an object if you would like, for example, a user.

When a user sends information through the tunnel(cable) to a channel, a method(action) will be called upon to do a certain thing. For example, save data a database, and then rebroadcast that data back out to anyone subscribed to the channel. This creates a very low latency connection between any number of users connected to a specific channel.

Perfect for the connection required to send the gyroscope sensor data between the mobile device and desktop device.

**Canvas: p5 Javascript Libary**

The game was going to be rendered on canvas for sure. It’s one of the strongest features of HTML5 today. Allowing for graphics and motion to be drawn which would be very hard to achieve otherwise. I started to learn the ways of HTML canvas and quickly realised that it’s going to be very hard to achieve the visual fidelity I wanted for this game using the javascript presented on the internet. So I searched for a canvas manipulation library which would take the work out of rendering all the assets that were required.

p5.js is a JavaScript library for creative coding. It has a full set of drawing functionality. However, you’re not limited to your drawing canvas. You can think of your whole browser page as your sketch, including HTML5 objects for text, input, video, webcam, and sound.

The basic premise of it is, you have a preloader method which is used to import all the assets into the sketch. After that, a setup function is called which contains all the code you want to execute to set up the sketch. Once that is all complete, you have a draw method which loops over the code inside of it at a certain number of times a second. Allowing for some pretty smooth processing of movements on screen.

**Physics: Matter.js**

All the elements within the game were to have actual physics allowing them to interact with each other. This would play a key gameplay mechanic in the final product. Searching for the right library on the internet made me choose the Matter.js javascript physics engine to perform all the physics calculations for this game. It is a very impressive library which allows for some very advanced simulations while using minimal processing power.

Check out some demos of what it can do! https://brm.io/matter-js/demo/#avalanche

I first designed assets I would use for the game, and then used a tool called Physics editor to trace the outline of the assets to create concave vector shapes which can be imported into Matter.js to allow for the physics simulation.

**Joining the canvas and physics together**

Creating a physics-based game brings a whole new meaning to object-oriented programming. Visualising the elements on the screen, and treating them as objects within the code, has made for some very functional programming.

Each part of the application that has a certain role is split up into ‘containers’. Aka, a leaderboard, or menu or game canvas can be split up into separate containers. Each container would have its own ‘Manager’ class which will be initiated on application startup. A managers job is to essentially provide functionality for that specific container, for example, sending POST requests for a form inside of the container, or displaying a notification inside the container displaying data received from a subscribed ActionCable channel. Additionally, an application should have an ‘adapter’ which contains all the functionality to connect with the database, acting as an interface for your Javascript container managers and the database. This is an object-oriented way of laying out the javascript code, for easier reading and future functionality implementation.

So it felt right to create a GameManager which would have the canvas as its container. This manager’s role will be to look after the state of the game. Create elements from a range of element classes. An ActionCable adapter was set up to act as an interface for the sensor data being received from the mobile device.

The element classes have the responsibility of creating a physical body inside of the Matter.js engine from a JSON file containing all the element vector coordinates. With all the physical body attributes held within the class instance, it will know its location within the physics engine at any time required. It will also have a method in which the p5 draw method can call on each loop, to draw an image on the canvas. So each element class holds information on the size, scale, position, texture, vectors, current state, health and a bunch of other information. But also has the special draw method which can be called on to draw itself on the canvas each frame. This brings the OOP coding you’ve done so far, to a new reality.

**Conclusion**

And hey presto! You have yourself a fully functioning realtime physics game being controlled over a realtime request protocol to the mobile device transmitting the realtime gyroscope sensor data being monitored! Okay… this is a very high-level overview of an example of what you could do. But hopefully, this inspires you to make something, right now, some might say, make something in real-time. Okay, maybe I’m the only person who has ever said that.

