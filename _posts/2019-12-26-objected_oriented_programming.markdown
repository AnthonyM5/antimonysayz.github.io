---
layout: post
title:      "Objected Oriented Programming"
date:       2019-12-27 01:15:09 +0000
permalink:  objected_oriented_programming
---


One of the most confusing and frustrating aspects of learning to code was learning about Objects in Ruby, and subsequently Object relationships, and how they can interact with each other, or in other words how Objects are associated with each other.  The most difficulty I experienced with associations was simply envisioning how each object is related, and specifically what pieces of information are available to each object.  

An important factor to remember is the Class setup in which each object is created, and which "getter" and "setter" methods are made available to each object that is created (initialized) in that class.  For example in the Has Many Objects Through Lab:



```
class Song 
	attr_accessor :name, :artist, :genre 
	@@all = []
	
 def initialize(name, artist, genre)
    @name = name
    @artist = artist
    @genre = genre
    save
  end
```

```
class Artist 
	attr_accessor :name, :songs
	@@all = []
	
	def initialize(name)
		@name = name
		@songs = []
		@@all << self
	end
	
```

Both classes can initialize objects that are associated to each other by the instance variables each class is able to access.  In the Song class, we see that they have access to an artist variable.  Upon creating a Song object, we have an artist variable that is set to an artist object from the Artist class, and similarly we have a song variable in the Artist class that can be assigned to a song object form the Song class.  This results in an artist object that can contain songs, and a song object that has an artist assigned to them.  

This provides a robust data structure with multiple applications, and information that is passed along mulitple object classes.  My initial frustration was understanding the direct relationships between the song/artist as it was not obvious to me that the arguments passed through the initialize method can also be objects that contain their own sets of information.  What was helpful was looking through the spec files and discovering exactly what was being passed through the argument, what the error was that was received, and what the correct format should have been.  From there it required careful and multiple passes of the code to figure out if there was a missing function, what exactly was being passed through the functions and try to isolate what the return value of each method inside the function was.  


