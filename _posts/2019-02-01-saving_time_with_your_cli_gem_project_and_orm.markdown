---
layout: post
title:      "Saving time with your CLI Gem Project and ORM."
date:       2019-02-01 21:21:14 +0000
permalink:  saving_time_with_your_cli_gem_project_and_orm
---


![](https://imgur.com/a/hFaTVJthttp://)


When you first start writing your own Classes in Object Oriented Ruby, you'll likely notice you are writing the same line, over and over. This usually can be avoided by writing more flexible, i.e. dynamic code. Two such methods would be #save and #create. You should never have to write these methods more than once! 

Below are some *instances* (pun intended) and ways to identify common patterns I fell into and how to avoid it. As the Co-Founder and Dean of FlatIron School, Avi Flombaum, is always telling us, "As programmers, you might remember, we are lazy. We don't like to repeat ourselves if we can avoid it." 

Objection Relational Mapping 

Do Not Repeat Yourself or DRY is an extremely useful principle in Software Development that seeks to reduce the amount of code written for a certain method or functionality. One way to reduce repetition is using through Object Relation Mapping (ORM). ORM will let us establish a connection to our database and manipulate the data, without having to write SQL. SQL is actually kind of fun to learn, but this will definitely get our apps up and running faster! 

Let's say I wanted to develop a web app for books, called GreatReads. In this originally named app, I have a Book and Author class. Step 1 for using ORM.

```
database_connection = SQlite3::Database.new('db/my_database.db') 
database_connection.execute("SOME SQL STATEMENT") 

```

After CREATE TABLE IF NOT EXIST for authors and books, we would want to do an insert. 

```
database_connection.execute("INSERT INTO authors (name, age, genre) VALUES ('Stephen King', 71, Horror) 
database_connection.execute("INSERT INTO authors (name, age, genre) VALUES('Joan Rowling, 53, Fantasy)
```'

To avoid repeating ourselves, we can create a .save method in our Author class to store a representation of our new authors in the database. 

```class Author 
 
  @@all = []
 
  def initialize(name, age, genre)
    @name = name
    @age = age
    @genre = genre
    @@all << self
  end
 
  def self.all
    @@all
  end
 
  def self.save(name, age, genre, database_connection)
    database_connection.execute("INSERT INTO authors (name, age, genre) VALUES (?, ?, ?)",name, age, genre)
  end
end```

Here we construct a class method that takes in name, age, genre, and database_connection. The db connection should be defined in your environment. The ?s in VALUES are called bind variables and they are bound to the placeholders in the query. In this case the ?s are bound to name, age and genre--which should come outside of the SQL Query. With SQL statements, we don't want to interpolate the values #{name}, #{age} for security purposes and to avoid SQL injections. See more [https://www.w3schools.com/sql/sql_injection.asp](http://).

Another area I learned how to abstract functionality, was in the CLI Gem Project. The goal of this project was to create a gem that could run in the terminal. Mine relied upon an API to get a user's request about the books that inspired the Game of Thrones TV series. My project had a Cli,API,Book, and Author class. This method is triggered when you run the gem by the #call method inside the CLI class. 

```
def self.start
     puts "These are your options, type them as is: "
     puts "print all books"
     puts "find character by name"
     puts "exit"

     input = gets.strip
     while input != "exit"
       if input == "print all books"
         print_all_books
         print "Enter another command: "
         input = gets.strip
       elsif input == "find character by name"
         character_name = gets.strip
         character = Got::API.find_character_by_name(character_name)
        if character != false
          puts "Character found! Type 'Y' for more info, otherwise type 'N'"
            y_or_n = gets.strip
              if y_or_n == 'Y'
                character.details
              elsif y_or_n == 'N'
                "Enter another command: "
                another_command = gets.strip
                if another_command == "find character by name"
                  character_name = gets.strip
                  character = find_character_by_name(character_name)
                elsif another_command == "print all books"
                  print_all_books```
									
Essentially the .start method puts out your options, then uses gets.strip to allow the user to input their option. The first time I wrote this, I didn't realize that when using a while loop, there's no need to use keep writing 
```puts "Enter anther command: " 
input = gets.strip```

...again and again. In my final version, I also abstracted my menu options into a class method, to allow the user to abstract away having to write the options a million times! 
  
```
def self.start
	input = nil
	while input != "exit"
		puts "What would you like to do? (type 'menu' to see options again)"
		input = gets.strip
		if input == "print all books"
			print_all_books
		elsif input == "find character by name"
			character_finder
		elsif input == "menu"
				menu
			end
		end
	end
	def self.menu
		puts "These are your options, type them as is: "
		puts "print all books"
		puts "find character by name"
		puts "exit"
	end
```

Now every time I am writing a method or conditional logic, I try re-factor my concerns into separate methods. My start menu should ONLY be printing out options and calling methods. That is, the CLI class should only puts things out and call other methods. Therefore, each class should have clearly stated concerns. You shouldn’t iterating over the Character class inside the CLI and you shouldn’t be puts in out string statements in anything other than the CLI class.  If you are heading into your CLI gem project, this is really important advice. If you don't follow this principle, your cohort lead will have you refactor it several times.


