---
layout: post
title:      "Flatiron's Collaborating Objects Lab"
date:       2018-11-23 19:21:34 +0000
permalink:  flatirons_collaborating_objects_lab
---


Roughly 5 weeks into Flatiron's Online Software Engineering Program, we delved into Object Relations. In this lab, we will be discussing how each Class works off of each other. 
Since we, like the rest of humanity, love music, we decide to design a music app to play and store music files. How will an instance of our MP3Importer Class send its files to create new instances of Songs? How will our Songs know which Artist they belong to? All of this will be covered by this lab, but I will focus on some helpful tools that helped me problem solve and refine my thinking. 


**Pro Tip**: Be sure to constantly refer back to each of the three `spec` files for this lab: `artist_spec.rb, mp3_importer_spec.rb, and song_spec.rb.` 


# MP3 Importer 

Ultimately, this Class will take in the files from the specs, parse them, and send them to the Song class to create new instances of Songs. Let's start with initializing the MP3 Importer class: 

`class MP3Importer
  attr_accessor :path

       def initialize(path)
         @path = path
        end

        def path
         @path
         end
	end `
	
	The path parameter will take in the file `path`to the MP3 files and the #path method will expose the files and allow us to read them. Remember that the attr_accessor for path is equivalent to writing:
	
 `def path
	   @path
	end
  def path=(path)
    @path = path 
  end`
	
	If you really want to understand where the songs are coming from, go to the mp3_importer_spec.rb file and see what path is equal to. 
	It's stored as a variable called test_music_path = "./spec/fixtures/mp3s". This is important to note for the next method, that will require a file path using Dir.glob. 
	
	#files
	
	For this method, FlatIron doesn't much guidance other than the suggestion to use Google. In order to load the files from the path directory and normalize the filename to only include the `.mp3`, we'll need to access the underlying file stream.  `Dir` is a class of objects that gives us such access and also allows us to change the directory into the one with our music files. Remember ours are in in: "./spec/fixtures/mp3s" which are stored as test_music_path in the spec files. 
	
The aptly named method `chdir` will change the directory to @path. Remember @path was initialized with the MP3 Importer Class above. 
	
	def files
    Dir.chdir(@path) do | path |
        Dir.glob("*.mp3")
      end
  end
	
Once inside, we want to access the global directory, only returning the files that contain the (".mp3") files. 'Globbing' files or using `Dir.glob` allows us to grab specific files in a directory by matching the pattern that we pass in --".mp3" and operate on them. 
	
 #import
 
 The sharpest and most effective item in my Ruby toolbox is  binding.pry. My Youtube Channel, will most likely be called PFFAPG, or 'Pretty Fly For a Pry Guy’. Go ahead and throw one into the `def import
 
 binding.pry
 
 end`
 
Don't forget to have `require 'pry'` at the top of your class too! I'll wait for you to do this. All done?! Nice job, let's applaud your follow-through. What is the return value of files? It’s important to see the array and study how the file is written. 

* With binding.pry you can eliminate the guess work about your return values, and immediately know what’s being passed into your method. 

What tools do we have to operate on each item in an array? If you answered the `.each` method, you're half-way there, straight up living on a prayer. #BonJovi

  `def import

    files.each do |files|
	
      #more code here 
    end
	
  end


end`

**Pop quiz**: Now that we are iterating through the files array, who's responsibility is it to create new songs? The Song class! 
Song.new_by_files(some_file) 

Now let's construct or Song class and the **#new_by_file method **

When we send our files to this method from the MP3Importer Class, our file will look like this: `"Action Bronson - Larry Csonka - indie.mp3".` We know that we want to save both the artist and song name, so how can we split them up? The #split method will separate each item of an array. The return value of the split method is an array, so how can we access the artist within the array if we have the following? Bracket notation to the rescue. Remember the first item within an array is always zero.

`artist_name = filename.split(" - ")[0]`

It's important that we assign the new array to the artist_name variable because we still haven't instantiated the new Artist. The rest of the method looks like this:

`def self.new_by_filename(filename)
artist_name = filename.split(" - ")[0]
    song_name = filename.split(" - ")[1]
   song = Song.new(song_name)
   artist = Artist.find_or_create_by_name(artist_name)
   artist.songs << song
   song
end`

The artist_name and song_name variables both contain the string that will be passed into the instances of artist and song. The reason I do this, is I like to cement my understanding by going into IRB or a binding.pry and create my own instances of Songs and Artist to see my creations come to life. This will reinforces abstract concepts and requires active engagement with your lab. Without doing this, you can waste hours getting stuck not knowing where your code is throwing errors or how to pin-point our bugs. 

