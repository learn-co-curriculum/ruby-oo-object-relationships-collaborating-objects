# Ruby Collaborating Objects Readme

## Objective

1. Gain a deeper understanding of object relations
2. Use other classes and methods within another class to collaboratively send messages to one another

## Introduction

Let's stick with our song/artist example. Our song class is responsible for handling songs. Our artist class is responsible for handling artists. However, theses things clearly have some relation to one another. Remember, a song belongs to an artist, and an artist has many songs. These two classes will have to collaborate. 

In fact, the classes do not even need to have any relationship (`"has many"` or `"belongs to"`) to collaborate. Imagine we have an MP3 Importer that is responsible for taking in a bunch of MP3 files and making a song for each unique filename. It is not hard to imagine that to make a song, the MP3 Importer will have to have some sort of communication with the Song class.

Let's take a look at each of these collaborations in more detail.

## MP3 Importer collaborating with Songs

The purpose of this MP3 Importer is to take in a list of mp3s and send each mp3 filename to the Song class to make a song. Let's just focus on the collaboration and assume our importer is successfully grabbing the filenames

```ruby
class Mp3Importer
  
  ... 
  
  def import
    # Assume we have an array of files coming from a method called #files
    files.each{|file| Song.new_by_filename(file)}
  end
end
```

Notice how *within the `Mp3Importer` class we are calling the `Song` class and a method within the `Song` class: `.new_by_filename`*.

When we hit this line of code, it will send us to the Song class to do whatever behavior we have defined in the `new_by_filename` class method. Then we will return to the `Mp3Importer` class to continue executing the code. This is at the heart of collaborating objects.  


## Songs collaborating with Artists

Since our song belongs to an artist, we will want to collaborate with the `Artist` class at some point. Imagine we have the following code:

```ruby
class Song
  attr_accessor :artist
  
  ...

  def artist_name=(name)
    self.artist = Artist.new(name)
  end
end
```

```ruby
class Artist
  attr_accessor :name
  
  def initialize(name)
  	@name = name
  end
  
  ...
  
end
```

The point of this code is that we want to be able to execute the following code given a song `hotline_bling = Song.new('Hotline Bling')` (Let's use Hotline Bling by Drake):

```ruby
hotline_bling.artist
```

And what you want it to return is not just "Drake", but an entire Artist object (an instance of the Artist class). Therefore, we call on `#artist_name=`, pass in the string, then send it off to the Artist class to make a new instance of an artist with the name of "Drake".

After `Artist.new(name)` finishes executing, we return to out `#artist_name=` method and assign that new artist object to out artist writer (`self.artist=`). Now we have successfully collaborated to create a `"belongs to"` relationship between a song and its artist.

Note: This, as you may have noticed, is problematic because we may end up with multiple artist objects for one artist if we pass in the string "Drake" more than once. Ignore this for now. Hopefully, this simplified version will help you see how collaboration occurs across multiple objects. 