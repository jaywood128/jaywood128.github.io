---
layout: post
title:      "Simplifying the Hashes Manipulation Lab Part I ."
date:       2018-11-09 13:49:10 -0500
permalink:  simplifying_the_hashes_manipulation_lab_part_i
---


 One of the more challenging concepts I've struggled with on my coding journey, is mentally visualizing what happens inside my code as it’s executed. Like most people who grew up in the 90s, when picturing the code that runs reality, I think of the streaming neon green data from the  *The Matrix*. How can we dive under the hood like Neo and Trinity?  When constructing our coding reality, we must get comfortable slipping into an environment that can pinpoint the exact line our code deviates off course.  Using the methods discussed bellow, we can focus on one of our most essential tasks on this journey: debugging our code. 

Luckily for us budding software engineers, there's the **'binding pry'** method. If you are drawing a blank on this kick-butt method, click here: http://pryrepl.org/ My goal in my first post is show how incredibly resourceful `binding.pry` is and how it can save you hours of guess work about why your code is not passing the .spec file.

![](http://i.imgur.com/8cZTBpE.png)

Inside of our nested hash we have seasons, holidays, and each holiday's respective supply. The first layer of the hash includes the season `:winter`, which points to => another hash of holidays. This is why it's nested. 

Our first job is to write a method that returns the second item for `:fourth_of_july`.  Before you start to write code, get into the habit of taking out paper and writing out everything you know, starting with the first Hash, holiday_hash. How would we access the supplies in the `holiday_hash` for the `:fourth_of_july`? What is supplies inextricably associated with inside the Hash?
Wrapping our supply is a [], so we know we are dealing with an array. Previously, we learned how to access specific indices inside an array by wrapping the desired index number in brackets. 

`fake_array = [ 8, 6, 7, 5, 3, 0, 9]

array[3]

=> 7`

To return the hash, let’s start by writing it into our method. The first thing we want to access is the `holiday_hash`. Beyond the first layer, what comes next? Inside the first hash contains a bunch of key values that are written as symbols. To return the contents inside a hash, simply write the name of the hash, and then wrap the name of the symbol you want to access: hash_name[:first_symbol]. So, what key do we want to access? If you answered the [:summer] key, yell 'huzzah!' regardless of location. 

```
holiday_hash[:summer]_________ #fill in the rest! What index are we trying to access? Holiday Hint: remember that the first index starts at 0. 
```

At this point, place a `require 'pry` at the top of your code. Then, just above the code you've written, place a binding.pry. It should look something like this. 

![](http://i.imgur.com/Y4hnDcv.png)


Our next Holiday assignment, is to write method that adds supply to both winter holidays. Think about what we are trying to accomplish. How many times would we have to add something to an array to accomplish this? How can we add to a data structure in an ordered fashion? Using the `.each` method will help us accomplish this. However, test what we will we have access to using `holiday_hash.each do |key, value|`.  Would this give us access to the supplies inside each holiday? Use `binding.pry` explore what you have access to inside that first .each method. 

Come back AFTER you finish this step. 

As a parting hint for the second test, I will say that there is a way to do this using 2 `.each` methods and one solution that only uses 1 `.each` method. If you want to add to the supplies using one, remember that to access the holidays in winter, we have to use bracket notation in the syntax of our `.each` method.  

Try running binding.pry, until you are able to return the hash with the array containing the supplies. 

Your solution should look something like this: 
`def add_supply_to_winter_holidays(holiday_hash, item)
 holiday_hash[:winter].each do |holiday, decorations|
   decorations << item
  end
end`
In the next post, we'll walk through the remaining tests! Good luck and remember to get help on your labs after 45 minutes of no progress. The technical coaches are great at guiding students, and will almost never spoon-feeding answers.

