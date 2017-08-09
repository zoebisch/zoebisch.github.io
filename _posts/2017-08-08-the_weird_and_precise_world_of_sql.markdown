---
layout: post
title:  "The weird and precise world of SQL"
date:   2017-08-08 17:20:59 -0400
---


Starting out with getting used to SQL queries can be a bit of a challenge!

Sure, the novel queries can be pretty easy and things are not what they seem sometimes. Making tables, inserting colums, SELECT * FROM table, are all pretty straight-forward. No problem.  You are whizzing along, learning in a flash. But then you realize:

**SQL has its dark side.**

It took me a bit before I realized how things like GROUP BY would squish my results.  What the heck was going on? I had one query in a lab that I kept going over and over and over and just wasn't making any headway.   Working through these mental gymnastics is really tough at first.  (Like in the [Learn.co lab ](http://https://github.com/learn-co-students/sql-crowdfunding-lab-v-000) that works with a crowdfunding database example, and the method to "selects_the_titles_and_amount_over_goal_of_all_projects_that_have_met_their_funding_goal"). To make matters worse, since there are "more ways to skin a cat" as they say, this further compounds the problem.

If you are working with SQL, the very best thing you can do is this:

**Stop what you are doing, and if you haven't already, go and dowload a database browsing tool!**

Seriously, do it.  It will all make a TON more sense when you start doing this.  I have been using "[DB Browser for SQLITE"](http://http://sqlitebrowser.org/). It's free, easy to use and works just fine, especially if you are learning.  

Armed with your new Database tool, you can now open the database you are working with and quickly run some queries to see what you are getting back.  What I suggest, in the beginning, is an iterative approach on the query to start finding what you are actually getting back.  You are not going to get anything less than that, nor anything else.  So if you are trying to "selects_the_titles_and_amount_over_goal_of_all_projects_that_have_met_their_funding_goal".

```
SELECT * FROM projects;
```

Now look back at the goal : selects_the_titles_and_amount_over_goal_of_all_projects_that_have_met_their_funding_goal.  Here is how I choose to break it down.  
1. Don't fall into seeing just writing in your SELECT statement first. Use the * . The reason for this is when you are learning how SQL JOINS and GROUPS, it's very helpful to see what is left over.  You can come back to that later.  Make this your last step, at least until you are familiar enough with the process.
2. Try playing around with what JOIN gives you in the DB Browser.  This will help you wrap your head around what is being given back. Note, in the case of a straight JOIN, try playing around with the ordering of left/right for the table:

```
SELECT * FROM projects JOIN pledges;

VS.

SELECT * FROM pledges JOIN projects;
```


3. Keep building in the complexity one step at a time and see what is coming back to you from the query:

```
SELECT * FROM projects
JOIN pledges ON projects.id = pledges.project_id
```

4. Now comes the mental juggle.  Things get a little weird when you invoke "GROUP BY".  Try different colums from each table that has been joined and observe closely what happens to your results.

```
SELECT * FROM  projects 
JOIN pledges ON projects.id = pledges.project_id 
GROUP BY projects.funding_goal;

VS.

SELECT * FROM  projects 
JOIN pledges ON projects.id = pledges.project_id 
GROUP BY projects.amount;
```

In the spirit of not giving away the lab answer, I will leave it at that.  I will however leave you with some final thoughts: 
You can do interesting things on your selectors (so long as the information has been properly JOINed and GROUPed BY 

```
SELECT pledges.amount - projects.funding_goal FROM  projects 
JOIN pledges ON projects.id = pledges.project_id 
GROUP BY projects.funding_goal;
```

Here you can see a mathematical operation on two different columns from two different tables that have been properly joined and grouped! You add this as *if it were one item to be selected*.  So you just separate with a comma after the operation:

```
SELECT pledges.amount - projects.funding_goal, projects.category FROM  projects 
JOIN pledges ON projects.id = pledges.project_id 
GROUP BY projects.funding_goal;
```


So if you are a little stuck, these pointers may come in handy!

Enjoy!




