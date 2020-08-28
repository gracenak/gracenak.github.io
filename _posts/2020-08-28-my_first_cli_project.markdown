---
layout: post
title:      "My First CLI Project"
date:       2020-08-28 04:31:56 +0000
permalink:  my_first_cli_project
---


We are 8 weeks into the Software Engineering Program at Flatiron, with my first CLI Project completed! The prompt for this project was to build a Ruby gem that provided a CLI to an external data source.

What is a Ruby gem? Ruby is the programming language and gem is the software package that includes applications and libraries  specific to Ruby. Ruby gems are essentially open sourced software libraries that other Ruby programmers can use to extend or modify functionality in their application. Some gems help automate tasks such as, 'pry', a debugging gem that helps you debug your code, or 'nokogiri', a gem that transforms an HTML webpage into ruby objects. 

The major components of a ruby gem:

1) **bin**: This is where I stored an executable file, allowing my program to run upon execution
2) **lib**:  This is where I stored my classes with my code
3)** README**: This contains a short description of my program, information about other files in the directory, install instructions, and a contributors guide
4) **Gemspec**: This contains information about the gem, including the files, my information as the author, and installed gem dependencies.


What is a CLI? It stands for "Command Line Interface." It is a text-based, user interface that executes automated tasks by accepting user inputs as commands. For example, when the user responds to the prompt, "Which concert would you like to know more about?", the user input of "1", "1. Juilliard String Quartet", commands a response/output of the data for the selected concert:

=======Juilliard String Quartet======
Date:

Wednesday, December 16, 2020

Teaser Info:

To celebrate the Beethoven year, the Juilliard String Quartet has commissioned German composer Jörg Widmann to write two quartets for the ensemble to premiere ...

-------- For More Info: --------
https://arizonachambermusic.org/events/juilliard-string-quartet-day-1/

How did my CLI provide this information? Web scraping is the process of extracting data from web sources. I required 'open-uri' and  'Nokogiri' gems so that my program could open, parse and transform the html/webpage into ruby objects. From there I could write code that iterated through each concert and extracted the specific data such as 'name', 'data', 'info', 'link', for each concert.

These 2 weeks were extremely challenging, yet exciting. I could write on and on about my process of coding my first CLI Project. What I really took out of this was continuing to understand the concept of Object Orientation Programming.  I am continuing to learn, practice, and grow in my knowledge and skills in coding. This assesment will be revealing of my knowledge and ability to communicate effectively. I am nervous and looking forward to it. Either way, I will learn to step in the right direction.
