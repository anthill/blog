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

1) it's a very hot day of August, and you're visiting a city. You're being cautious and drink a lot of water to avoid dehydration. Too bad, you're in front of a statue spurting, and now you badly need to pee.

![Yeah, he's teasing you]({{ site.baseurl }}/assets/article_images/device-agnostic-bladder/Bruxelles_Manneken_Pis.jpg "Manneken Pis in Brussels, Belgium. Myrabella / Wikimedia Commons / CC-BY-SA-3.0")

2) you're hanging out with friends at your favorite pub, drinking quite a few beers. From the second pint on, you've been going to the toilets every half hour, because, well... beers. Now you're on your way home, and it's already been half an hour since you last peed.

![Drink responsibly !]({{ site.baseurl }}/assets/article_images/device-agnostic-bladder/Beer_newcastle.jpg "Beer, A pint of beer...Barcamp North East 2 Sunday evening, Drink. Author: Tim Dobson. Wikipedia Commons")


Sometimes, you are strong enough to hold your wee, sometimes not. And you end up buying a coffee you'll never drink just to use a restaurant's bathroom, or finding yourself a hidden spot between two cars to unleash the stream.
Neither of these last minute solutions is really acceptable, being absurd or non-civic, and you wish you'd know where a public toilet actually stands.

This is a very common situation, and **providing a simple map of the public toilets is not good enough**. 

**This toilet problem is solved by accessibility and design.**


## Accessibility
*Everyone should have the right to pee.*

In the case of our city, Bordeaux, such apps are already available. On Android and iOS.

Only.

Now you do have a phone with Android or an iPhone, and you'll be ok. But what about phones with Firefox OS, or Windows Phones? What about future devices with OS that haven't been created yet?

This discrimination based on the OS/device has no reasons to be. Such **applications of common need should be available to anyone, regardless of what phone they like best**. Of course, one solution is to develop as many applications as there are different OS. But we don't want to do that, it would clearly be a waste of time.

All smartphones have a web browser. So by **developing a web app once**, and with very little extra code, it will be available to all.

 The basic need is pins on map. With web browsers, this can be achieved in **2 hours for all platforms**. Using the data from [opendata.bordeaux.fr](http://opendata.bordeaux.fr), combined with [OpenStreetMap](http://www.openstreetmap.org/#map=5/51.500/-0.100), [Mapbox](https://www.mapbox.com) and [Leaflet](http://leafletjs.com), it's actually pretty easy to pin the toilet locations on a map layer.

 But this is not enough.

 Remember, you desperately need to pee. And the brand new app you've just downloaded gives you points on a map. Okay, you can read a map, you could find out where you are and which toilet is closest to your position. But you are not really in the mood for brain work right now. **The information you need ("WHERE IS THAT GODDAMN TOILET ?"), you need it fast.**

## Design
*This is where the UX kicks in.*

* We want the user to be able to find the closest toilet **as fast as possible**. Or maybe the 3 closest, just in case one is occupied or broken. Other toilets are not really relevant.

* The toilet you're looking for needs to be **one you can use**. You should be able to discard urinals if you're a girl, or to find only accessible toilets if you suffer from physical disability.

* Also, you don't really need to know the exact address, you just want to know **the way to go**, and your ETA (Estimated Time of Arrival)

![5 minutes and 25 seconds should be ok]({{ site.baseurl }}/assets/article_images/device-agnostic-bladder/screenshot.png "Screenshot of what the app looks like")

We came up with a design that fits reasonably enough those requirements. The app displays the map with the toilet pins. Then it tries to localize you. If successful, it finds the 3 closest toilets. Then requests are sent to Google Directions, which calculates itineraries. The app displays the calculated infos (path, distance, and time), and adapts the zoom level to fit the 4 important points: your position, and the 3 closest toilets.
The app also gives you the possibility to filter toilet types, and reprocesses according to your request, always giving you the toilet you most probably need.

If you're just being curious, you might to want to check how much time it would take you to go pee to the opposite side of the city. You can access that information by clicking the pin you're curious about. And back to the actual useful information clicking on your position.

This is, to our sense, an appropriate trade-off between having lots of features while keeping a minimal design. **This kind of app shouldn't be doing much more, and certainly not any less**.

We are aware that there are limitations in this app. For instance, it is only available for Bordeaux. But we see this app as a proof of concept. The next steps would be to collect toilet data from around the world, clean the datasets, and bundle it all in the app. We adapted the app for Paris just to see how much effort it would take us to make it possible. Not much more than 1 hour. So maybe in the future, we'll make this more universal.

You can find the apps at:

* [http://ants.builders/ToilettesBordeaux/](http://ants.builders/ToilettesBordeaux/)

* [http://ants.builders/ToilettesParis/](http://ants.builders/ToilettesParis/)

If you want to contribute to this project, checkout
[this GitHub repository.](https://github.com/anthill/ToilettesBordeaux/)

## Oh wait, one last thing !
*WebApps can look like native apps.*

You might not like the fact that this app is embedded inside your browser. Fair enough, we understand that. **All mobile browsers have a way to install a standalone version of the web app** just by clicking on the "Add on desktop" button (or equivalent). Then you'll get just the app. No more browser. Just the app. And you'll always have the most recent update, without doing anything.

**This is what the web should be: useful and accessible, to everyone.**

<iframe class="vine-embed" src="https://vine.co/v/OrPUOZDTIJD/card" width="600" height="600" frameborder="0"></iframe>
<br/>


