---
layout: post
title:  "Craigslist Scraping, Ruby Gem CLI part 2"
date:   2017-07-22 23:53:31 -0400
---


Sometimes, you just don't know what you are getting into!

Last I left you, dear reader, I was in the midst of structuring my Ruby Gem CLI project, having forged ahead through my (first) IP ban.  I kept moving ahead, somewhat slowly.   After the first ban, I kept working with a single page call at a time. This is the safe zone for CL and was able to work on all my methods and deal with the unexpected without having to wait out a ban release. 

Boy did my program grow in scope!   With CL and this project there really is no in-between.  I mean you either go for it, or you pick a different site.  At the end of the day, I had to work through a few other bans before I got the timing down between page calls.  Since CL is binned into 120 items per page (I am fairly certain this is standard), we have to hit CL with a page call for every item page list.  There is no getting around this, at least to my knowledge. It seems 5 to 8 seconds between scrapes is a reliable time delay.  For added measure, I use an array of user_agent strings, which are essentially data which tells the site which browser the request is coming through. This is transparent to us when using a browser, but is a key item when working with scraping some sites, and is just a part of how modern browsing works.  With a little help from my Learn Instructor who pointed me to a great article on Web Crawlers, I decided to incorporate a random user_agent string as a secondary method to help prevent a ban.  More about user_agent can be [found here](http://https://en.wikipedia.org/wiki/User_agent).  

Since I have had the two methods functional, I have been successful at avoiding a ban.  Another tactic, which I had tried and was successful with, was to route the traffic through a free proxy.  The issue with that was, duh, it was still a static IP.  I ended up getting that IP banned too, whoops. I came to find out that is not an uncommon tactic.  A future idea, and something I do want to create, is a class that locates free proxies and creates a pool and then selects a random one for a page call. 

Back to the project scope.  Yup. It got out of hand. But in reality there was no other way I could see doing this project.  It was either both feet in or not at all.  I ended up with a pretty flexible program.  It successfully can scrape any category from the CL "for sale" menu. The menu items themselves are scraped as well, since why not? Tricky part there, which was an added several hour excursion, was a few of the categories have a second level of even more categories. So last minute before submission, I worked through all of that.  A little rushed add-on but it's functional and possibly could be cleaned up. All of the individual items become objects and we work with those objects.  The scraper class does the heavy lifting, the item class takes care of creating and merging (more on that in a minute) item data, the "Price_Manager" class does all the user interaction and selection and is the interface between the Item data, Scraper and user.  A bin file kicks everything off and the concerns file contains all of the methods used for searching, sorting, printing, statistics and merging. 

Once you have selected a category, the proper links are scraped incrementally (hmm a lightbulb just went off on something CL *might* be looking for) and returns the first list of information. Once there the user can search for a item (string), look at the basic stastics, price range and even drill down into the third level of the site using a unique identifier CL calls "pid".  If you select an item by pid, the program pulls the data and merges it with the proper Item object with any of the variable attributes.  It also keeps track of what category the item originated from and the url. This isn't so important right now but for future expansion, this information will be key in keeping track of things. It's not uncommon to have thousands of items from a scrape on a CL site in a big city. 

Part of the trickiness with this was there are certain things that have to be done before the user can do another thing. Just like you can't run a media player if your OS isn't loaded, you can't select a category until you have scraped the categories.  You can't search for an item until you have selected a category. You could select one but without an items to look for you won't get anything. So rather than write more code to return something that makes sense (like a warning) I decided instead to force the user to run first the category, followed by the item to provide instanciation.  This was a design choice and could very easily be switched around depending on what one would chose to do with the code from here on out. 

I had originally embarked on working with some more advanced statistics on the data.  But because the whole project just got pretty big, I decided to focus on making everything clean, functional and (hopefully) using best practices, which is the real goal of this project.  

In the end I spent waaay too much time on this, but I am proud of it.  I think it works great and is way more quick to use than CL with all of the visual clutter.  I have already been having A LOT of fun, just to see what's out there. Things like $20,000 perfume evaporators, Metallica tickets, Antique stools and even an $800 Star Wars Destroyer!  

**Speaking of tickets, this is a great way to find them**.  What makes this so nice is you can quickly juggle through the data for your price.  I hope to add some kind of fuzzy price logic because hey, after all you wouldn't want to miss seeing Radiohead because the search cut you off at $100 when the price was $105, now would you? 

So, I hope you can find it useful too. 

[https://github.com/zoebisch/craigslist-price-it-right](http://https://github.com/zoebisch/craigslist-price-it-right)
