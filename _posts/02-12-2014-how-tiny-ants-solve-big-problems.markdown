---
layout: post
title:  "How tiny ants solve big problems"
subtitle: "What we call collective intelligence"
authors:
  - name:   Romain Crestey
    twitter:    https://twitter.com/OursonEnPeluche
    github_image: "https://avatars0.githubusercontent.com/u/8417814?"
    bio: Musician, drawer, gamer, engineer, Romain is a creative geek, convinced that the best technology is half useless without a well-thought design. With a growing UX interest, he works on developing ergonomic solutions to help give the users back the control on their data.
  - name:   Alexandre Vallette
    twitter:    https://twitter.com/vallettea
    github_image: "https://avatars3.githubusercontent.com/u/371303?"
    bio: Data-scientist, founder of ANTS, his passion for machine-learning applied to geographical data and networks comes from his Phd in chaos theory. Open-data enthusiast, he is committed to show how open-innovation can lead to a better governance and economy.
date:   2014-11-25 14:34:25
categories: post
tags: 
  - collective intelligence
  - design
  - front end
  - fun
image: /assets/article_images/how-tiny-ants-solve-big-problems/cover.jpg
credits: Geoff Gallice from Gainesville, FL, USA - Army ant bivouac, CC BY 2.0
---

We "spent" a week creating our [animated banner](http://ants.builders). Not only because it was fun and we wanted to learn Javascript, but also because **we wanted to express what's deep inside our vision of ANTS**.

Ants are very limited individually but, together, with simple rules, they can achieve what seems impossible at first.

For example, the picture on the top of this article represents a colony on bivouac : each individual grabs its neighbor, holds tightly and they eventually form a solid sphere similar to a football. A colony of individuals usually perceived as liquid is now one solid object! Why? To protect their Queen in the middle while they move the anthill to another place. 

Another **incredible sign of collective intelligence is the ability for ants to find the shortest path between a source of food and the anthill**. Finding the shortest path is one of the most difficult problems in graph theory. When you use Google Maps to find your way it is because you can't compute the shortest path with only a map in your hands. And behind all these routing applications lie a bunch of beautiful algorithms (Djikstra, A*, contraction hierarchies) that took decades for the best mathematicians of the world to figure out.

An ant can do something with only a few neurons that the human brain is not able to achieve without a smartphone? No, **ANTS** can. The group.

How does it works? 

Ant scouts wander around, randomly. If ever they find a drop of sugar, they will take as much as they can (with pleasure, we'll assume) and try to find their way back to the anthill. Their way back is far from being the shortest but to be sure not to lose their sugar, they leave traces of pheromones all along the path. Pheromones are chemical products that can be seen as perfume, and in this case, act like roadsigns. 

When another ant scout randomly crosses a path that smells of pheromones, it knows that there is food at the end. So it follows the path and finds the food. Once filled up with sugar, it goes back to the anthill like the first ant scout did, but the pheromones have evaporated a bit so the second ant follows the initial path with some variations. This process repeats on and on with thousands of ants: longer paths are less used, smell less, thus their roadsigns are less visible, and so they are slowly abandoned. To the contrary, shorter paths accumulate more and more pheromones and thus continue increasing their smell, making the roadsigns grow. Rapidly the shortest route among stones, grass, and hills is found.

Our banner represents 4000 ants moving on a network (the network is invisible but merely fills all the screen). Ants are moving along one edge, but arriving at the end of it, they must choose another path among the ones connected to their current edge.

![The ants move on a mesh, and the pheromones help them find the shortest path to food]({{ site.baseurl }}/assets/article_images/how-tiny-ants-solve-big-problems/ants_mesh.jpg "Ants mesh")

There is only one single rule for each ant to decide which edge to choose:

- flip a coin: if heads, take a random path
- if tails, choose the smelliest.

On the edges representing the letters, we've put high-class honey. See what happens!

We are not entomologists, so we're not really here to discuss insects. But what does strike us and what we want to share, is this vision: ***so much can be achieved with good collaboration***.


- **simple rules**: they're key to fast execution and freedom of movement;
- **communication**: no hierarchy in communication. Everyone should be able to talk to anyone else without any intermediaries. Intermediaries are bottlenecks and tend to concentrate power;
- **distributed power**: each ant is free and can choose to take another path. This is the key element for finding the optimal path. 
- **resilience**: look what happens when you hover your mouse on top of these ants. Working as a colony is almost unstoppable.

Let's keep that in mind for our organization.


