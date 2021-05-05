---
layout: post
title:      "Understanding DOM and The Three Pillars of Web Programming"
date:       2021-05-05 09:29:09 +0000
permalink:  understanding_dom_and_the_three_pillars_of_web_programming
---


Spending the past 6 weeks learning JavaScript has been quite a journey. Building my first JS application has been fun and challenging, clarifying a lot of the foundational and technical aspects of JavaScript. The biggest challenge has been piecing it together, creating a JS Frontend Application with a Rails API backend, demonstrating the Client-Server Communication flow.
The Flatiron School's Three Pillars of web programming has served as a mantra throughout the learning and building process.
Recognize Events
Manipulate the DOM
Communicate with the Server

In other words, when an event happens (ie: "click", "submit"), what kind of fetch request do I want to make(ie: "GET", "POST" "PATCH", "DELETE") and then how do I want to manipulate the DOM(ie: "render a new post", "render a like", "update a post", "delete a post").
DOM (Document Object Model) is the foundation to front end programming. It is the middle layer that updates the content based on events in Javascript. The DOM is created when the page loads the HTML from the browser provided from the server. The browser reads the HTML along with the CSS and JS defined in <script> tags to create the DOM inside the browser. The browser then uses the the DOM object to create and render the page. The DOM is an object oriented representation of HTML, making up the web page.
DOM programming uses JavaScript to manipulate the DOM: finding, adding, removing, and adjusting elements. JavaScript defines functionality of the website, while HTML defines the structure of the website, and CSS defines the visualization and style of the website.
JS Events listen for events that occur in the browser, such as a "click" or "submit". When JavaScript recognizes an event attached to an event handler, that "handler's work" is executed, stored in a function callback.

We identify where the event should listen, which then sends a message to the server, indicating success to update the DOM. We identify by making AJAX calls, requests to the server without reloading the web page which then works with the data received from the server. The DOM returns HTML and CSS, followed by JavaScript to fulfill the rest of the action behind the scenes.
AJAX calls rely on Promises, JSON(a serialization format that can be stored, transmitted and reconstructed later to send a collection of data), which is all abstracted into fetch. Fetch does not return the actual object but a prototype of the object data source. AJAX allows you to pull in dynamic content and retrieve data from multiple sources.
We use JS to send web requests to the API to receive a collection of JSON to return on the DOM. We take this representation of these objects and manipulate the DOM but rendering and updating with the "fetched" JSON data.
Whenever in doubt in web programming, follow the Three Pillars to guide you into understanding the flow and retrieval of JSON objects, serialized representations of the objects. This mantra will continue to guide me as a software developer.
