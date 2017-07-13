---
layout: post
title:  "Craigslist scraping, IP bans and other adventures in programming"
date:   2017-07-13 04:55:20 +0000
---


So the current challenge at Flatiron is to make a functional website scraper, that works down to two layers deep. This is a big deal project, and is assessed. So, wanting to put my best foot forward, instead of rushing into a project, I thought for about a day on what I wanted to create.  I had been working a lot with Wikipedia on a side project and at first thought about doing that...but then I recalled there was a video lesson which had some of that in there.  So, staying away from safe waters, I started pondering some more. I had been selling some items on Craigslist (CL) and had been thinking about how price is really the key to any sale.  Too high and people scoff, too low and people immediately think there is something wrong with your listing.  But there is that sweet spot, "market value" as they say, that puts you right into the zone where you can actually make a sale.  After all, I thought, I am spending all this time looking at postings on CL and sorting through everything was a little painful. I mean, I had even listed a printer that was already there with two other listings for the exact model...but I never saw them until I had relisted the item and done a more specific search.

And then it hit me. Hey, let's have Ruby do it!

Things were off to a great start.  I was humming along building out the OO framework for the vision, everything was coming together. Before long I was able to pull in items from a category, classify them, do some basic statistics on them (low, high, mean).  I just wanted to get a solid functional program before I started working out the ideas for expanding and refining.  So I had been working with a page at a time.  Realizing that sooner or later I would have to pull in *all the listings*, I hit a stride in functional code that was a good breakaway point to start incorporating this functionality.

So I put together a simple looping structure that parsed the base url into each page of 120 items, up to the total number that I would need for the maximum listings and after a little bit more effort, I was breezing through all the items.  Things were looking up!  

I then added a new top level ability to glean all the listings on the main sale page, the idea being that I would incorporate this into my CLI menu. 

I started noticing, that things seemed to go a little slower.  Requests for each page were taking a few seconds. I started digging around a bit, perhaps there was some optimization I was missing?  Maybe I wasn't using Nokogiri properly? Nothing. So I brushed this off, a bit bothered because realistically it shouldn't take that long to gather 2500 data listings. 

And then ***IT*** happened.  CL banned my IP. 

Ouch!

I sent a letter to the email address listed on the screen:

> blocks-b@craigslist.org
> date:	Wed, Jul 12, 2017 at 4:29 PM
> subject:	Hi!
>I am not doing anything bad!
> 
> I am writing some code for a project that is for my school.  I am learning how to scrape, and decided a good project to help me use Craigslist more effectively would be to find similar items to the one I am trying to sell and find the price range and such so that I can establish a market price.  Would you please unban my IP, I have no malicious intent! I also have a bunch of listings at the moment that I can't access now that I have for sale. 
> 
> Thanks

Frustrated, but determined, I started digging. Hindsight being 20/20, of course it made sense, in some ways.  I started reading dread tales of companies being taken to court by CL for scraping. Was I now a criminal? Had I truly gone to using my powers for the Dark Side?  I had already confessed! I'm going to internet jail, where all of the guilty and not so guilty like Michael Riconosciuto go.  Thankfully, no. They were only after people who were making money off of scraping CL in a big way.  

So how to prevail? 

Proxy? 

So I started digging and found a nice Gem: Socksify.  A little bit and some help from the internet, and I was back up and running! Except, until that is, that CL banned the free Proxy IP.  Ouch! 

Was this it for my project? Would all that time be spent making a program that, in the end, was useless? No! I won't let it be so!  

More digging, and a few revelations. My first thought was, ok so make another scraper to grab the IP pool for free proxies and then use that to bypass the watchful eye of the firewall.   But I also started thinking about the problem itself and my experience with the funny behaviour, and when it started occurring and then it hit me: Time makes all the difference.  I knew in the back of my head that CL's API had only become suspicious once I was sending too frequent requests.  This was a dead giveaway, that again with that good-ole hindsight vision, I could have seen plain as day.  Of course it all makes sense....now.  I found a repository on GitHub that showed me a magic number, if you want the details, you'll have to rummage through my code ;).

I now have my IP back, without having to take my router offline until my ISP issues a new one. I am not sure if they just do a temporary ban or if someone listened to my plea in the email. 

At any rate, I hope to be tacking this thing down soon. 

Stay tuned for part two. 
