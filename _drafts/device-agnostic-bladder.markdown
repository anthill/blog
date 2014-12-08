---
layout: post
title:  "Device agnostic bladder"
subtitle: "Providing accessibility through the web"
authors:
  - name:   Romain Crestey
    twitter:    https://twitter.com/OursonEnPeluche
    github_image: "https://avatars0.githubusercontent.com/u/8417814?"
    bio: Musician, drawer, gamer, engineer, Romain is a creative geek, convinced that the best technology is half useless without a well-thought design. With a growing UX interest, he works on developing ergonomic solutions to help give the users back the control on their data.
  - name:   David Bruant
    twitter:    https://twitter.com/DavidBruant
    github_image: "https://avatars1.githubusercontent.com/u/165829?"
    bio: David is a leader in front-end development, especially in javascript and standards specification. Contributor to Mozilla, he believes deeply in open web and open source. He teaches coding to young children and datavisualization in design schools.
date:   2014-12-08 15:52:25
categories: post
tags: 
  - toilets
  - accessibility
  - web
  - mobile
image: /assets/article_images/device-agnostic-bladder/cover.jpg
credits: toilet, Martin Abegglen, Barcelona, CC BY-SA 2.0
---

Who never felt the despair caused by the urge to pee while not being able to ? Yeah, not that many fingers raised...

Well, if you think about it, this is most often the consequence of two situations:

1) it's a very hot day of August, and you're visiting a city. You're being cautious and drink a lot of water to avoid dehydratation. Too bad, you're in front of a statue spurting, and now you badly need to pee.

![Yeah, he's teasing you]({{ site.baseurl }}/assets/article_images/device-agnostic-bladder/Bruxelles_Manneken_Pis.jpg "Manneken Pis in Brussels, Belgium. Myrabella / Wikimedia Commons / CC-BY-SA-3.0")

2) you're hanging out with friends at your favorite pub, drinking quite a few beers. From the second pint on, you've been going to the toilets every half hour, because, well... beers. Now you're on your way home, and it's already been half an hour since you last peed.

![Drink responsibly !]({{ site.baseurl }}/assets/article_images/device-agnostic-bladder/Beer_newcastle.jpg "Beer, A pint of beer...Barcamp North East 2 Sunday evening, Drink. Author: Tim Dobson. Wikipedia Commons")


Sometimes, you are strong enough to hold your wee, sometimes not. And you end up buying a coffee you'll never drink just to use a restaurant's bathroom, or finding yourself a hidden spot between two cars to unleash the stream.
Neither of these last minute solutions is really acceptable, being absurd or non-civic, and you wish you'd know where a public toilet actually stands.

This is a very commun situation, which can be easily solved by providing a map of the public toilets in your city, and let people install it on their phones. To be totally honest, in the case of our city, Bordeaux, this app was already available. On Android.

Only Android.

Now you do have a phone with Android, and you'll be ok. But what if you have an iPhone with iOS? Or a phone with Firefox OS, or a Windows Phone?

This discrimination based on the OS/device is quite problematic. Such applications of common use should be available to anyone, regardless of what phone they like best. Of course, one solution is to develop as many applications as there are different OS. But we don't want to do that. It's clearly a waste of time.

What is common to all modern phones is a web browser. These all work basically the same way. Develop once, and make it available to all.
So why not make a WebApp ?

With the data from opendata.bordeaux.fr, it's actually pretty easy to pin the toilet locations on a map layer provided by, say, Mapbox, or equivalent. But this is not quite enough. Remember, you desperately need to pee. And the brand new app you've just downloaded gives you points on a map. Okay, you can read a map, you could find out where you are and which toilet is closest to your position. But you are not really in the mood for brain work right now. The information you need ("WHERE IS THAT GODDAMN TOILET ?"), you need it fast.

This is where the UX kicks in.

* We want the user to be able to find the closest toilet as fast as possible. Or maybe the 3 closests, just in case one is occupied or broken. Other toilets are not really relevant.

* The toilet you're looking for needs to be one you can use. You should be able to discard urinals if you're a girl, or to find only accessible toilets if you suffer from physical disability.

* Also, you don't really need to know the exact address, you just want to know the way to go, and your ETA (Estimated Time of Arrival)

![5 minutes and 25 seconds should be ok]({{ site.baseurl }}/assets/article_images/device-agnostic-bladder/screenshot.png "Screenshot of what the app looks like")

We came up with a design that fits reasonably enough those requirements. The app displays the map with the toilet pins. Then it tries to localize you. If successful (don't complain if you're in a tunnel :p), it finds the 3 closest toilets (with a regular euclidean distance). Then requests are sent to Google Directions, which calculates itineraries. Then the app displays the calculated infos (path, distance, and time), and adapts the zoom level to fit the 4 important points: your position, and the 3 closest toilets.
The app also gives you the possibility to filter toilet types, and reprocesses according to your request, always giving you the toilet you most probably need.

If you're just being curious, you might to want to check how much time it would take you to go pee to the opposite side of the city. You can access that information by clicking the pin you're curious about. And back to the actual useful informations clicking on your position.

This is, to our sense, an appropriate trade-off between having lots of features while keeping a minimal design. This kind of app shouldn't be doing much more, and certainly not any less.

You can find it at this address : [http://ants.builders/ToilettesBordeaux/](http://ants.builders/ToilettesBordeaux/)

Oh wait ! You might not like the fact that this app is embedded inside your browser. Fair enough, we understand that. All mobile browsers have a way to install a standalone version of the web app just by clicking on the "Add on desktop" button (or equivalent). Then you'll get just the app. No more browser. Just the app. And you'll always have the most recent update, without doing anything.

This is what the web should be: useful and accessible, to everyone.
