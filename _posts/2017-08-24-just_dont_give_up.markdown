---
layout: post
title:  "Just don't give up!"
date:   2017-08-24 00:07:48 -0400
---


So going back and forth between Sinatra, Active Record, Erb, Html, all starts to get a little blurry when you are experiencing it for the first time. 

I was plugging away and then hit the dreaded wall. You know, the seemingly tiny little thing, like a grain of sand that you spend forever trying to get out your eye, but no matter how hard you try, no matter what you do, it's not coming out. I started feeling this way on the [Sinatra Playlister Lab](https://github.com/zoebisch/playlister-sinatra-v-000). I was at a really solid pace, things were coming together. I had a little bit of a struggle getting the joins table relationships worked out, but once that clicked it felt good.

I started working on my routes and they were doing great. One by one, I was making good progress, and felt pretty good in my understanding. All except for one part. 

This stuff:

```
  <input type="text" name="song[artist_id]" id="Artist_Name" value="<%= @song.artist.name %>" ></input>
```

Seems simple enough right? But what exactly is this id anyway? Just some odd value? No, this is just what you know from CSS. It is an id selector. For some reason, that never clicked in my head.  I mean it all seemed so new, and with so many other things going on, it seemed one small detail that you just "needed".  Ahh but Capybara will drive you mad if you don't get this.  This is what Capybara seems to be looking for, at least in the cases I have been exposed to so far. Do yourself a favor, and if the test tells you that it bombed like so:

```
    Capybara::ElementNotFound:
       Unable to find field "Artist Name"
```

What this means is you haven't done the following:
```
  <... id="Artist Name" value="...>
```

Simple enough, unless you don't understand why. 

So what about harnessing the power of Active Record to automatically take care of all the behind-the-scenes database and object updating?   You will want to do this, really you will.  It's one thing to getting it working, but a totally different thing to make it right.  

What I mean by this is, that I had gotten to the last few tests on the lab, and just couldn't get my song's genre's to update.  No matter what I tried (and I tried so many things) it never seemed to work.  So, needing a reach out for help, I had a session with an instructor.  We walked through everything in that section and he pointed out a few key items that would have been hard to discover on my own.  You see, I knew that my inputs to the update method were wrong, but I just couldn't see how.  So we worked to get some formatting on my erb inputs, changing song to song[names] and a few other small items.  

It was great, my tests passed!  I was done! Phew. 

So I fired up Shotgun and went to my seed database.  

And that's when the REAL fun began.  Ok I was passing my tests so why wasn't my song edit method not working??? This quickly devolved into a cat-and-mouse hackathon where I tried everything under the sun.  On both sides of the equation, I worked with the routes and with the erb.  Nothing. It was always coming up short, usually with what Active Record expecting an object and getting a hash, or an array.  But I just couldn't "SEE" into the code. What the heck was really going on??

I thought about submitting that lab.  Really. I mean I had passed, right?

Hell no. This would haunt me.  I am about to get to the second project and I need to know my stuff.  So into Shotgun. I can't tell you how many iterations this took me, but I finally hit the jackpot. It's a secret.  I will share it with you.

You will never get Active Record to update without sticking to the attributes that you have prescribed in the table migration!  Try it, go into a pry session and ask for:

```
Song.attribute_names
```

For me this spit out ["id", "name", "artist_id"]

So coming full circle, we look back at what we are asking through erb post. 

```
  <input type="text" name="song[artist_id]" id="Artist_Name" value="<%= @song.artist.name %>" ></input>
```

This part: song[artist_id], or alternatively when looking to update the song[name] this is where name= comes in. Whatever is pulled in through value=, gets assigned to name= in the resultant hash.  So, when we get back into the route and examine params, we should see something like this:

```
{"song"=>{"names"=>"That One with the Guitar", "artist_id"=>"Person with a Face", "genre_ids"=>["2"]}}
```

In this case, when we use the update method, our hash must be lined up with our object model.  So, if I were to use name="song[artist_name]", Active Record would produce an error like this:

```
pry(#<SongsController>)> @song.update(params[:song])
ActiveRecord::UnknownAttributeError: unknown attribute 'artist_name' for Song.
from /Users/zoebisch/.rvm/gems/ruby-2.3.1/gems/activerecord-4.2.6/lib/active_record/attribute_assignment.rb:59:in `rescue in _assign_attribute'
```

**unknown attribute 'artist_name'**. So mapping backwards, because the attribute does not match, Active Record has no idea how to locate what you are asking.  

So tldr; set up your erb name= value in the inputs to **match** the attributes in your corresponding Class method, organize your hash to be nested according to the structure you are after (i.e. song[name], song[artist_id], song[genre_ids][]), and for the Love of Mike, please use the update method. You will thank me later.

One quick aside...when you pass in a hash that contains an array of ids, you will want to pluralize your name= from the base value, to it's plural form, i.e. genre_id becomes genre_ids.  That is the magic of passing multiple values in and letting Active Record do the heavy lifting.

Cheer!
