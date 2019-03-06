---
layout: post
title:      "Undefined Method for nil:NillClass "
date:       2018-12-13 12:47:37 -0500
permalink:  collaborating_objects_and_undefined_method_for_nil_nillclass
---



![](http://i.imgur.com/XGrLxn3.jpghttp://)


Having never fully understood Ruby's `No Method Error`, it certainly haunted me when I had to explain my code in the Object Relationship Lab to my co-hort lead. To put it optmistically,I felt lost in the endless Hang Sơn Đoòng caves in Vietnam. Never been, but my ESL students assure me it's worth the trip. 

While this lab and problem solving exercise was frustrating, I'd like to stress this point if and when you are struggling a ton with error messages: get use to them, embrace them and don't immeditaley ask a friend or your Google search bar for a solution you won't fully understand, or be able to reproduce. Read documentation and Stackoverflow, ofcourse, but try to resolve your own errors. Remember, when the day for your techincal interview  arrives, you won't be phoning any friends.  
There are points where you will need to be told what to write, but the more you push yourself, the more you will internalize what you've learned for the next time and you will VERY likely see this error messsage again. If you're new to Flatiron, these techincal-style interview questions with your cohort lead will be really helpful for landing a job. While frustrating, I am thankful that my lead purposely pushed me to explain things. Thanks, Dakota!

This post will not cover everything in the Collaborating Objects Lab, except how to fix the class method save_by_filename.

A NomethodError is raised when a method is called on a reciever that can't be called on that object. Take a look at the following code and see if you can spot my mistake. 

```
def self.new_by_filename(filename)
  
    artist_name = filename.split(" - ")[0]
    song_name = filename.split(" - ")[1]
   song = Song.new(song_name)
   artist = Artist.find_or_create_by_name(artist_name)
   artist_name.name = artist # source of my no method error
   artist.add_song(song)
   song
 end
```
 
 
 Here too are the test expections for song_spec.rb file for the method new_by_filename with my error message bellow.

```
describe '.new_by_filename' do

  ```
  it 'associates new song instance with the artist from the filename' do
      Artist.class_variable_set("@@all",[])
      new_instance = Song.new_by_filename(file_name)
      expect(new_instance.artist.name).to eq('Michael Jackson')
      expect(Artist.all.size).to eq(1)
      expect(Artist.all.first.songs.empty?).to eq(false)
    end
```
```
		
		
	
		
```
1) Song .new_by_filename associates new song instance with the artist from the filename
 Failure/Error: expect(new_instance.artist.name).to eq('Michael Jackson')
 NoMethodError:
	 undefined method `name' for nil:NilClass
```

The reason for the nil:NillClass artist.name = artist_name is because artist_name will return a string "Michael Jackson". In this case. the `name` property can only be called on Artist object.


So artist.name = .....can you figure it out? The name property is the caller and artist is the receiver of the method. So if you get undefined method 'example' for nil:NillClass, ask youself two questions: 

1) Did I define my method and include the right attr_accessors and 

3) 2) 2) Can my method or property be called on this object? If not, which object should be able to access or utilize this method? 

***Remember that songs belong to an Artist and an Artist has many songs.*** 

With  artist.add_song(song), the artist will know about it's songs. The add_song method is inside my arist.rb. So if the artist knows about it's songs, what's relationship is missing? To test your knowledge about Undefined method for nil:NillClass, try to correct the code bellow and get it to pass. 

```
  def self.new_by_filename(filename)

    artist_name = filename.split(" - ")[0]
    song_name = filename.split(" - ")[1]
   artist = Artist.find_or_create_by_name(artist_name)
  song = Song.new(song_name)
   artist.add_song(song)
   song_name.artist = artist # why won't this line work? What is the artist property being called on? 
   song
 end
```


