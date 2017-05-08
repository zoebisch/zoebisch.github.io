---
layout: post
title:  "A Taste of HTML5 Semantics"
date:   2017-05-08 20:03:07 +0000
---


I was hoping to have been involved with something more challenging with HTML5 by now, but honestly it's all pretty straight-forward. So since I couldn't find something "tricky", let's talk about the Semantics in HTML5.  These are some really useful new markups that were compiled by the good folks at Google and some others based on the most frequently used patterns from the past.  That's always a fantastic starting point, sifting through historical data to find trends and creating emergent tools to make a little more sense of things.  So, let's get started, shall we?

One of the best things about the inclusion of Semantics is that you can still create webpages from previously used concepts, and indeed your site will likely still contain them.  However, you'll be missing out on some really great elements that basically reflect overall collective human (web) design patterns and best practices if you choose to overlook these.  Besides, your work will become dated and it's important to stay relevant in the world of design.

Here is the shortlist, taken from [The World Wide Web Consortium (W3C)](http://https://www.w3schools.com/html/html5_semantic_elements.asp):

```
<article>
<aside>
<details>
<figcaption>
<figure>
<footer>
<header>
<main>
<mark>
<nav>
<section>
<summary>
<time>
```

These should be apparent as to their usage.  No hidden meanings here and if you are familiar with any well structured document, you will naturally be able to immediately make use of them. Interestingly enough, on that argument alone, one could argue that Google and friends merely proved a pattern that humans have already established.  After all, without knowing anything about HTML and the Internet, would any of those words appear foreign to anyone familiar with a high-school level document?

Nonetheless, let's have a look at some of the media related ones:

`<video>`    Use this element to encapsulate Video within the site.  The great thing about this is that you can add controls, loop               the video, preload the data and even add a poster to the beginning of the playback within the media player.  You can even autoplay or use the muted attribute to further tailor your content.   Additionally designers can control the width="desired width" and height="desired height". A key thing to note is that we will still need to add multiple file formats to provide support for browers:

```
              <video width="my_width" height="my_height" controls> <!--  opening tag, size and contols -->
                 <source src="my_video_file.ogg" type="video/ogg"> <!--  Chrome, FireFox, Opera -->
                 <source src="my_video_file.mp4" type="video/mp4"> <!--  All major browsers-- >
                 Your browser does not support the video playback  <!-- Fallback in case of no support -->
              </video> <!-- video semantic closing tag. -->
```

`<audio>`  Use this element to encapsulate Audio within the site.  Usage is nearly identical to `<video>`, but controlling width or height is omitted because it is not applicaple to an audio file:

```
              <audio controls>                                      <!-- opening tag and controls-->
                 <source src="my_audio_file.ogg" type="audio/ogg">  <!--  Chrome, FireFox, Opera -->
                 <source src="my_audio_file.mp3" type="audio/mpeg"> <!--  All major browsers-- >
                 Your browser does not support the audio tag.       <!-- Fallback in case of no support -->
              </audio> <!-- audio semantic closing tag. -->
```


In both these cases, fallback content can be text or even a link to the original, or a stable source. 

`<track>` And if you are using these elements, you should know about `<track>`. `<track>` allows designers access to specific visual text tracks within media. This is an extremely useful Semantic and should be incorporated in any explanation of the new media Semantics. [From W3C](http://https://www.w3schools.com/tags/tag_track.asp): "This element is used to specify subtitles, caption files or other files containing text, that should be visible when the media is playing." Usage is as follows, using our above example:
```
              <video width="my_width" height="my_height" controls> <!--  opening tag, size and contols -->
                 <source src="my_video_file.ogg" type="video/ogg"> <!--  Chrome, FireFox, Opera -->
                 <source src="my_video_file.mp4" type="video/mp4"> <!--  All major browsers-- >
                 Your browser does not support the video playback  <!-- Fallback in case of no support -->
	             <track kind="captions" src="subtitles_en.vtt" kind="subtitles" srclang="en" label="English">
              </video> <!-- video semantic closing tag. -->
```

Here, we have specified an english language subtitle track (of type .vtt) that is considered kind="chapters".  A little more on "kind" can be found on the [Mozilla Developers Network](http://https://developer.mozilla.org/en-US/docs/Web/HTML/Element/track) but basically it distinguishes beteween the navigable sections found in a video layout, namely: subtitles, captions, descriptions, chapters and the (non visible to user property) metadata. 

I hope this has been a good introduction to the value and importance of the Semantics within HTML5. 


