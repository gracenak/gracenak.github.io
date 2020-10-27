---
layout: post
title:      "Creating a Sinatra Application"
date:       2020-10-22 20:41:48 -0400
permalink:  creating_a_sinatra_application
---


It was so much fun to create an application that I would like to use in the future (with improvements and enhancements as I learn more about software engineering and continue to feed my knowledge of coding, of course). 

When tax season arrives, I am always scrambling through my inbox of expense receipts, bank statements, and meticulously sifting through emails and my planner to recount the freelance work that I did for the year. Could I be more organized? Always but this was something that I felt was useful and close to home.

What is Sinatra? It is a domain specific language in Ruby that allows you write web applications. It rides on the middleware of Rack, the software that bridges the connection between every request and response. 
This Sinatra app built on MVC Framework is a system that has seperation of concerns and helps build web applications simply and dynamically.

How does this work?

1. The browser makes an HTTP request. The Sinatra **controller** (ie: app_controller.rb)  takes that request and routes it to to the appropriate action (ie: /users)

2. This action then requests these objects from the **models** (ie: user.rb) 

3. The **model** requests for the data from the database

4. The database responds and then the models send the objects to the action

5. The action sends the objects to the **view** for rendering (ie: erb: welcome)

6. The **view** responds with HTML

7.  The **controller** takes that HTML and sends an HTTP response

**M**odels are the logic in this framework, where we have classes. This is where the data is created, manipulated and saved.

**V**iews are filled with code that we render in the browser when requested. These pages with HTML will display text in the browser. We call this the front end of our application because it interacts with the user directly.

**C**ontrollers dictate the action, going between models and views to implement the requests.

The application_controller.rb serves as the interface and flow of the application. The configure block tells the controller where to look and find the views and public directory(containing my css styling and more). We enable sessions, which enables the app to use the **sessions** keyword to access a session hash. The session hash stores the user's id. 

What is the importance of sessions? Because HTTP is a stateless protocol, meaning it will not retain any information about a user during the time of the request, it is important to set up sessions so that it can retain user information at the duration of the session. This session begins from the time when the user logs in. Any data stored in a session hash can be accessed  during the time of the session. Logging out will remove the user id and clear the data from the session hash. How cool is that? 

Next, we set our session_secret, 'password_security'. This adds an extra layer of security and protection for the user. 

Our gem, bcrypt stores plain text passwords as a salted, hashed version so that only the user has knowledge of the password. They store this password in the database in a column called "password_digest". Our macro, "has_secure_password", works in conjunction with bcrypt and does not store the plain text password in the database, but as an encrypted password that can not be decoded. This maco has a built in **authenticate** method thats takes in an argument of password_digest. This is why we can ask 
**if user && user.authenticate(params[:password])** to authenticate the user's params at login.


Helper methods are accessible in views and provide logical support, such as logged_in? and current_user methods.

Our **config.ru** is our executable file. It is responsible for loading our environment and code.  We mount "Run ApplicationController" to start the application and additionally mount other controllers that inherit the ApplicationController  (ie: use UsersController) in order to load functioning code for those controllers. On top of those controllers, we mount Use Rack::MethodOverride to make patch and delete requests. We need Rack, our middleware, to intercept for every sent and received request via PATCH and DELETE. This is why our edit and delete views have this form:

<form method="POST" action="/gigs/<%= @gig.id %>

< input type="hidden" id="hidden" name="_method" value="PATCH" >


We use rack to override method and as stated in input, has a name of "_method"  with a value of "PATCH".

The most important concept implemented was object relational mapping of these models through "has_many" and "belongs_to". A user "has_many" gigs. A gig "belongs_to" a user. A gig has a foreign key of user_id and a user has a primary key of id.

class Gig < ActiveRecord::Base

  belongs_to : user
	
end

 class User < ActiveRecord::Base
 
   has_many : gigs
	 
 end



**@user = User.find(1)**



**User id: 1**, username: "gracenak", email: "gracenak@gmail.com", password_digest:
"$2a$12$.271nUL/TGRbjipErtnV8epqLdap1gUM3Y30n.llcCO..."


**@user.gigs**

Gig id: 35, employer: "Bella Strings", date: "2020-10-13", description: "1/11 -1/15. 5 services $150/service", payment: 0.75e3, expenses: 0.5e2, **user_id: 1**>, #<Gig id: 38, employer: "True Concord", date: "2020-10-12", description: "Chamber stuff", payment: 0.5e3, expenses: 0.17e2, **user_id: 1**>, #<Gig id: 43, employer: "Arizona Friends of Chamber Music", date: "2020-10-20", description: "Prokofiev Quintet", payment: 0.2e3, expenses: 0.5e1, **user_id: 1**

**@gig = Gig.find(35)**

**Gig id: 35**, employer: "Bella Strings", date: "2020-10-13", description: "1/11 -1/15. 5 services $150/service", payment: 0.75e3, expenses: 0.5e2, **user_id: 1**

**@gig.user**


**<User id: 1**, username: "gracenak", email: "gracenak@gmail.com", password_digest: "$2a$12$.271nUL/TGRbjipErtnV8epqLdap1gUM3Y30n.llcCO..."


Building this application was a lot of fun. I am looking forward to the next module and project!









		
		




