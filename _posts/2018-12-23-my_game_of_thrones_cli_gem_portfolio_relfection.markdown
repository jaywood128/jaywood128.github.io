---
layout: post
title:      "My Game of Thrones CLI Gem Portfolio Relfection "
date:       2018-12-24 01:14:37 +0000
permalink:  my_game_of_thrones_cli_gem_portfolio_relfection
---


While I could've probably just scraped a website, I ended up building my CLI project by making requests to an Application Programming Interface (API) which is basically a tool to access the hashes nested inside an array. 
My API was An API of Ice and Fire(https://anapioficeandfire.com/)
This API is an open one - which means authentication is conveniently not required to request data from the API. 

Here's a taste of what an API request will return from this URL with following address: 
 hash = {
    "url": "https://anapioficeandfire.com/api/books/1",
    "name": "A Game of Thrones",
    "isbn": "978-0553103540",
    "authors": [
      "George R. R. Martin"
    ],
    "numberOfPages": 694,
    "publisher": "Bantam Books",
    "country": "United States",
    "mediaType": "Hardcover",
    "released": "1996-08-01T00:00:00",
    "characters": [
      "https://anapioficeandfire.com/api/characters/2",
      "https://anapioficeandfire.com/api/characters/12",
      "https://anapioficeandfire.com/api/characters/13",
      "https://anapioficeandfire.com/api/characters/16",
			........
			}
The array of characters is quite lengthy so I tried to limit each book to 20 characters. Each book is a hash and all the books are inside an array. By iterating over the array and the accessing each property with hash["url"] , you can map over each property and make sure that each new instance of a Book is initialized with each property. 
			
Creating new instances of Characters actually took me the longest time. Ultimately, I was able to refactor my code into two separate methods, one that collected the character_url and another that saved the instance of Character based on the url passed in. 

One thing I would warn anyone about this project is don't create relationships between objects if you don't absolutely have to; get the data your CLI requires and keep it simple. Honestly, I struggled a lot on this project. I spend days where zero progress was made, despite my best efforts. Ultimately, I spent way more time building this than was outlined in the Syllabus. I know I learned some valuable lessons, ultimately the limited coaching I received was probably a good primer for coding in a real professional environment, without any handholding. 

While I did not finish my project the way I wanted to, I know that for me to have done this on my own, using the site I choose, was probably next to impossible. However, I was able to create Book objects and Characters objects with the attributes and data provided by the API. I learned an incredible amount about trial and error, error messages, the difference between .map and .each, and so much more. 




  
