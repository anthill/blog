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
image: /assets/article_images/how-tiny-ants-solve-big-problems/cover.jpg
credits: Geoff Gallice from Gainesville, FL, USA - Army ant bivouac, CC BY 2.0
---

We "spent" a week creating our [animated banner](http://ants.builders). Not only because it was fun and we wanted to learn javascript, but because **we wanted to express what's deep inside the our vision of ANTS**.

Ants are very limited individually but, together, with simple rules, they achieve miracles.

For example, the picture on top of this article represents a colony on bivouac : each individual grabs its neighbor, holds tight and they eventually form a solid sphere similar to a football. A colony of individuals usually perceived like a liquid is now one solid object ! Why ? To protect the queen in the middle while they move the anthill to another place. 

Another **incredible sign of collective intelligence is the ability for ants to find the shortest path between a source of food and the anthill**. Finding the shortest path is one of the most difficult problem in graph theory. When you use Google Maps to find your way it is because you can't compute the shortest path alone only with a map. And behind all these routing applications lie a bunch of beautiful algorithms (Djikstra, A*, contraction hierarchies) that took decades to the best mathematicians of the world to figure out.

An ant could do with its few neurons what we can't figure out ? No, **ANTS** can. The group.

How does it works ? 

Exploratory ants are wandering around, randomly. If ever they find a drop of sugar, they will take as much as they can (with pleasure we guess) and try to find their way back to the anthill. Their way back is far from being the shortest but to be sure not to lost their finding, they leave, all along the path, some pheromones. Pheromones are chemical products that can be seen as perfume. 

When another exploratory ant, randomly crosses a path that smells pheromones, it knows that there is food at the end. So it follows the path and finds the food. Once filled up with sugar, it goes back to the anthill, as the first ant, but the pheromones have evaporated a bit so it follows the initial path but with some variations. This process repeats on and on with thousands of ants: bad portions of the path are less used and thus smell less so are slowly abandoned. Shortest bit enable more back and forth and accumulate pheromones and continue increasing their smell. Rapidly the shortest route among stones, grass, hills is found.

Our banner represents 4000 ants moving on a network (the network is invisble but merely fills all the screen). Ants are moving through edges but arriving at the end they must chose another edge among the one connected to their present edge.

![The ants move on a mesh, and the pheromones help them finding the shortest path to food]({{ site.baseurl }}/assets/article_images/how-tiny-ants-solve-big-problems/ants_mesh.jpg "Ants mesh")

There is only one single rule for each ant to decide which edge to chose:

- flip a coin: if head, take a random path
- if tail, chose the smelliest path.

On the edges representing the letters, we've artificially put some sugar. See what happens !

We are not entomologists and not really here to speak about insects. What strikes us and what we try to share, is the what can occur with good collaboration.


- **simple rules**: are the key to fast execution and freedom of movement.
- **communication**: no hierarchy in communication. Everyone should be able to talk to anyone else without any intermediaries. Intermediaries are bottlenecks and tend to concentrate power
- **distributed power**: each ant is free and can chose to take another path. This is the key element for finding the optimal path. 
- **resilience**: look what happens when you hover your mouse on top of those ants: working as a colony is almost unstoppable

Let's keep that in mind for our organisation.


