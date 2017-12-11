---
layout: post
title:      "They don't teach you THAT in school "
date:       2017-12-11 15:43:39 +0000
permalink:  they_dont_teach_you_that_in_school
---


The process of learning can  be both direct and indirect.  What I mean by this is, when a course has a curriculum and tests, you are learning directly or perhaps more specifically, you are being taught a direct concept.  An example of learning indirectly would be being given a task that requires you to solve a problem.  Along the way you run into the unexpected which requires you to call upon both your familiar skills but also to probe deeper on your own to understand.

I felt this way today.  While I was busily framing out my Rails Portfolio Project for The Flatiron School, I was cruising along with making good progress on the models.  I had my associations down, validations on key data and even decided to venture into some rspec testing for the first time...and decided to get the user authentication and third source user authentication through facebook.

Then things got weird.

At first, using the patterns I had learned in the curriculum, I had established a Devise user, added the appropriate methods, routes and other intricacies  needed to setup the flow when logging in using facebook. Things looked fair so I fired up the server and began checking out what I had so far.  

My first error was because I had improperly added my whitelisted route on Facebook, a nod to not trusting every refresher you find online.  The site was specifically adding facebook, yet failed to include facebook in the route like so instead of the proper `http://localhost:3000/users/auth/facebook/callback` they had shown it as `http://localhost:3000/users/auth/facebook/callback`. Thankfully, since Rails is fairly verbose when reporting errors, I was able to track it down.  Interestingly, if you omit 'http', facebook will default the route to 'https', which I suppose makese senese but sometimes the devil is in the details...

After fixing that, I was able to get through the first portion and was executing the first part of the process and then I kept hitting "The action 'facebook' could not be found for Devise::OmniauthCallbacksController". All references I could find kept pointing me back to an improperly coded route.  However when I would review my code, I was **sure** I had it right:

```
devise_for :users, :conrollers => { :omniauth_callbacks => "users/omniauth_callbacks"}
```
Whatever I seemed to do, whatever I changed kept giving me the same error.  I was really started to get worried because this was taking far too long.  And then I spotted it. Look closely at the code above and maybe you'll spot it too. Since there is no such thing as a "conroller", well you get the idea. I fixed that like so:

`devise_for:users, :con`**t**`rollers => { :omniauth_callbacks => "users/omniauth_callbacks"}`

All was well with the world for that brief moment when I finally leapt over the action error and ran into my third and final issue using devise with omniauth. This one was a little more obscure.

If you follow all of the conventional sources on setting up authentication with facebook using omniauth, I will tell you there is one gotcha that might occur.  In the user model, where we setup the class method to instantiate the user from the callback, it specifically uses email to signin.  What you might not know is that you can signup for facebook without an email.  Yup. You can use a phone number, so the method does not handle that.  I stubbed out a TODO to show you what I am talking about:

```
  def self.from_omniauth(auth)
    where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
      #TODO: facebook will accept a PHONE NUMBER as an EMAIL for signup, handle exception in future
      user.email = auth.info.email
      user.password = Devise.friendly_token[0,20]
    end
  end
```

So when a user signs up with a phone number and your model is based on email, well there is a problem.  Fortunately because I knew that I had signed up via phone number, it clicked fairly quickly once I got down into a pry session.  

Well, there you have it, sometimes we learn things indirectly by doing.  I would say this is what makes the curriculum at Flatiron so strong,  in that you have the rigor of test driven development that you have to keep plugging away at until you get it right and on the other hand, you have open-ended exploratory projects that both test your knowledge and allow you to soar to new heights on your own. 
