# Ruby Collaborating Objects Readme

## Objective

1. Gain a deeper understanding of object relations
2. Use other classes and methods within another class to collaboratively send messages to one another

## Introduction

Hello Nina!!!

Let's stick with our song/artist example. Our song class is responsible for handling songs. Our artist class is responsible for handling artists. However, these things clearly have some relation to one another. Remember, a song belongs to an artist, and an artist has many songs. These two classes will have to collaborate.

In fact, the classes do not even need to have any relationship (`"has many"` or `"belongs to"`) to collaborate. Imagine we have an MP3 Importer that is responsible for taking in a bunch of MP3 files and making a song for each unique filename. It is not hard to imagine that to make a song, the MP3 Importer will have to have some sort of communication with the `Song` class.

Let's take a look at each of these collaborations in more detail.

## MP3 Importer collaborating with Songs

The purpose of this MP3 Importer is to take in a list of mp3s and send each mp3 filename to the `Song` class to make a `Song`. Let's just focus on the collaboration. Our `MP3Importer` class will receive a list of filenames that look like this "Drake - Hotline Bling". `MP3Importer` will then send each of those filenames to the `Song` class to be created.

```ruby
class Song
  attr_accessor :title

  def self.new_by_filename(filename)
    song = self.new
    song.title = filename.split(" - ")[1]
    song
  end

end

class MP3Importer
  def import(list_of_filenames)
    list_of_filenames.each{ |filename| Song.new_by_filename(filename) }
  end
end
```

Notice how *within the `MP3Importer` class we are calling the `Song` class and a method within the `Song` class: `.new_by_filename`*.

When we hit this line of code, it will send us to the `Song` class to do whatever behavior we have defined in the `.new_by_filename` class method. Then we will return to the `MP3Importer` class to continue executing the code. This is at the heart of collaborating objects.


## Songs collaborating with Artists

Since our song belongs to an artist, we will want to collaborate with the `Artist` class at some point. Imagine we have the following code:

```ruby
class Song
  attr_accessor :artist

  # other methods

  def artist_name=(name)
    if (self.artist.nil?)
      self.artist = Artist.new(name)
    else
      self.artist.name = name
    end
  end
end
```

```ruby
class Artist
  attr_accessor :name

  def initialize(name)
    @name = name
  end

  # other methods

end
```

The point of this code is that we want to be able to execute the following code given a song `hotline_bling = Song.new('Hotline Bling')` (Let's use Hotline Bling by Drake):

```ruby
hotline_bling.artist_name = "Drake"
hotline_bling.artist
```

This should then return the new `Artist` object that was created by the `#artist_name` method.

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/ruby-collaborating-objects-readme' title='Ruby Collaborating Objects Readme'>Ruby Collaborating Objects Readme</a> on Learn.co and start learning to code for free.</p>
