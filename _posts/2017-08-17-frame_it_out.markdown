---
layout: post
title:  "Frame it out!"
date:   2017-08-17 03:00:40 +0000
---


Shifting gears into Sinatra and absorbing the key concepts can be a little tricky at first.  Here's what I did in the coursework up to the "Sessions" section to keep from going off track.

A lot of new concepts are going to be thrown at you if you are working through the [Flatiron Web Developer Curriculum](http://https://flatironschool.com/programs/online-web-developer-career-course/) once you get to the Sinatra section. Don't worry too much about the details at first, it will start to come together once you get to the Nested Forms Lab Review. (Trust me on this one)! 

The first thing that is a little weird is the Embedded Ruby, but a lot of that just boils down to some new syntax.  In fact a lot of this stuff you are well prepared for, once having completed your CLI Gem Project.  The OO should not be a sticking point by now and hopefully the mental wiring is all in place.  If not, I would suggest really looking back and retracing some of your steps.  

Just like someone hired to build a house doesn't have to be told about framing and what tools are needed, a programmer embarking into working with Sinatra must have a thorough command of OO Ruby or the most they will be able to do is follow the framework of others.  Learn does a good job of structuring its students, so I would be very surprised if you hit the Sinatra lessons without having a deep grasp of OO Ruby. Same thing goes for database relationships and migrations.  This is all going to come together for you soon!

Some of the tidbits are introduced and not expounded on, like yield.  In the Ruby OO lessons yield was explained but left up to you as a programmer to decide if you like it.  I personally use it when it makes sense, and it's a very powerful tool to help towards the "DRY" objective. We again see a yield concept in the Sinatra lessons, but don't get too hung up on it. Know that it is there and how to use it, but don't think (like I did) that the very next lesson is going to test your understanding of it. 

Instead, as you progress through, make sure you really grasp these items: 
1. Model View Controller (MVC) and Forms.
2. Routes and Dynamic Routes.
3. Forms. 
4. Did I mention forms?

So MVC is kind of self-explanatory.  The key here is mapping the idea in your head to the code.  One important aspect is where things live.  That is a key reflection of the flow for the program, which always is, but becomes especially important with Metaprogramming and DSL's that do things like automatically look for where corresponding route erb files live. So it's best to just interrelate these concepts in your head, and keep practicing. 

It's building in that scaffolding that makes the project come alive, and honestly it's a bit of repetition of form.  Just do the same thing, the same way, over and over.  If you keep doing this, THE RIGHT WAY (as our good friends at Flatiron have laid out for us), you will end up learning the foundations for the REST method. It's just that simple. Just like we used specific naming conventions when doing database migrations, the same idea comes into play here.  Although, one big difference is, if you alter the naming conventions with Sinatra, Sinatra won't care, it will still work, unlike migrations which are looking for exact key words when auto-generating the database from the migration file. So, I would advise disciplining your code to follow the convention: plurals for when you are working with more than one, singular when you are working with one, names reflecting the path (and/or visa-versa) and objects that reflect form (params) data.  It will all flow together nicely if you focus on doing so.   

So don't overcomplicate things, and try not to get frustrated.  Oh speaking of which, Capybara is really specific and is likely going to get under your skin.  Yes, mind your spaces! 
 
Also, a huge help is to read your error codes, especially the ones Sinatra throws at you.  "You don't know diddly"...er sorry, "Sinatra doesn't know that Diddy" (my version is stuck in my head lol) is really helpful. Especially the backtrace.  It's A LOT of information, but take your time. It is telling you most of what is going on.  

For instance, before I got to the video review, I was having one heck of a time figuring out the syntax to get the path to the '/pirates/new.erb' file.  And although I had to reach out to an instructor, which took like 1 second to show me what I was doing wrong, I did at least know from the backtrace that the path was wrong because it was looking for 'new.erb' in the directory above /pirates. Like so:
```
<Errno::ENOENT: No such file or directory @ rb_sysopen - /Users/zoebisch/Development/code/sinatra-nested-forms-v-000/views/new.erb>
```

So, I knew it was just down to how I was trying to pass in the path.  What I couldn't get after a bunch of searching and hacking away at it, was the format.  I tried:  erb :'/pirates'new, erb "pirates/:new" and a bunch of other silly things...hey to a hammer everything looks like a nail, or, er, something like that?!?

I'll give it away, only because it was so simple I had to laugh. To properly pass a path within the views directory:

```
    erb :'subdirectory/embedded_ruby_file'
		
		i.e. 
		
		erb :'pirates/new'
```
 
I realized too, as I moved deeper into the lessons, that I was adding some stuff in that isn't particularly necessary.  Kind of like when you write a SQL statement in a Heredoc in Ruby, you can omit the termination character ";" you don't need elements such as html, body, nor DOCTYPE html within your erb.  Embedded Ruby is sharp enough to work around all that, but if you need them, they are there.  It would seem you  just don't have to include them in most cases. 

Until next time, cheers and Happy Coding!
