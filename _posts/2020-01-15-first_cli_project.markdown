---
layout: post
title:      "First CLI Project"
date:       2020-01-16 02:19:08 +0000
permalink:  first_cli_project
---


The concept of a Ruby Gem can be daunting, and admittedly is still a little confusing after this excercise.  The idea of a Ruby 'gem' is a way to distribute programs and libraries - they can be full fledged programs like our CLI project, that can be executed and installed on various local machines, or they can be helpful libraries like the 'nokogiri' scraper gem that has built in functionality to open HTML pages, and extract data.  

The idea of making a scraper program is at the core of data management and interpretation.  Our goal is to implement a realtively simple tool that would extract data from a website, and return the text into your terminal in a quick and responsive way.  I decided to build a scraper tool that would scan a website containing trading information about Crytocurrencies, and return data such as: Current Price, Current Volume, Mineable Status, etc. when prompted by the user.  

Initially we wanted to get a good understanding of setting up the environment to build our gem, and have the right dependencies in each of our files to make sure that we are storing the data in a comprehensive way so that our data can be collected into an object, and each object has shared functionality that can ultimately power functionality in the user's terminal.  It is important to understand what kind of data you are extracting, and how you want each part of the data to be stored and called upon later.  This will allow us to write a simple line of code in our console file that would execute the program, and have the dependent parts of the code interact seamlessly to gather all necessary data.  For example the console file that starts my project
```
CLI.new.call
```

This is effective and to the point.  Once this file is run, the CLI class would create a new instance and initiate the 'call' method from that object instance.  This allows us to abstract all functionality into an organized way.  The call functionality in my case initaites a chain of methods that call upon my Coins class that stores all the data, and allows me to isolate specifically what to display to the end user depending on their input.  To make everything neater, we can put all of our required file paths into a single file and have all subsequent files associate to it.  

The fun part then is to determine where in the HTML your data lies, how to isolate each piece of data and assign it to an attribute for your objects so that it can be called individually.  For example my Coins class creates an object and assigns these attributes:

```
attr_accessor :name, :price, :volume, :market_cap, :circulating_supply, :change_24
```

The value for these attributes come directly from a hash that is created from my Scraper class:

```
coin_info = {
        name: coins.css(".cmc-link").first.text,
        price: coins.css(".cmc-link")[1].text,
        volume: coins.css(".cmc-link")[2].text,
        market_cap: coins.css("div")[2].text,
        circulating_supply: coins.css("div")[3].text,
        change_24: coins.css("div")[4].text
      }
```

This way I can then display each attribute individually just by calling on a Coin instance, with the dot notation associated with that attribute.  

The beauty of the Ruby programming language is that even the code can be made to seem logical, and the methods can be written in such a way that it offers an explanation of what exactly is happening, making it seem simple enough!
