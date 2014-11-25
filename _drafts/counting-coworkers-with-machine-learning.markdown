---
layout: post
title:  "Counting coworkers with Machine Learning"
subtitle: "Hacking the Node, part 1"
authors:
  - name:   Armand Gilles
    twitter:    https://twitter.com/arm_gilles
    github_image: "https://avatars3.githubusercontent.com/u/8374843?"
  - name:   Alexandre Vallette
    twitter:    https://twitter.com/vallettea
    github_image: "https://avatars3.githubusercontent.com/u/371303?"
date:   2014-11-25 14:34:25
categories: post
tags: featured
image: /assets/article_images/counting-coworkers-with-machine-learning/cover.jpg
---
Le node c’est quoi
Le problème et amélioration
Les contraintes du projet (privé etc..)
Rasberry 
( Graph rendu du projet sur une journée )


## Networking footprint
- Intro : Comment ça fonctionne ()
- On ne sniff pas les paquets et pourquoi 
- Qu’est ce qu’on récupère et le résultat 
(Photo : Rasberry)

==> On a juste besoin d’un Rasberry et d’un wifi dungle


## Computer vision

Let's talk about some computer vision now. So what is a humain for a computer ? Well it's a pretty hard question and that's is the difference between you (the fact that you know their is a difference between a humain and a chair) and many number and no learning. 

Therefore, you have to feed your learning database's pictures with some postive pictures of humain (crop of upper body). thereafter you have to indicate what isn't a humain. For this part, I took some pictures of our cowerking space with no body inside and randomly crop them.

Ok you have the first step, now time to play with opencv's parameters ! Training a model can be easy, build a good one can be long and choose the right one need some skills.

<iframe width="800" height="600" frameborder="0" seamless="seamless" scrolling="no" src="https://plot.ly/~babou/62.embed?width=800&height=600"></iframe>

Here is a benchmarck of model I tested. you can read it as if the box's model is below 0, your model find more humains as he should and conversely. Build a model is like an adventure and the mean progress (red line) shows it. You have to wonder why I don't stop my model making with the gray one (test 27) with a mean of 1.14, it seems pretty good! Well calculate difference between a picture with 8 humains and 8 positive matchs is a good indicator. But your 8 matchs can be falses positives and it was the case...

So I decided to improve my database picture's. I added many positive (131 to 600) and I crop manualy some negative (606 negative). In fact in my previous models, I saw many false positive match with bag, coat on chair and shoes . So I crop it to feed my new model and to teach him what isn't a humain. I build a new one with my custom made picture (600 postive / 606 negative) and the result was catastrophic (test 33). 

In my previous model, I randomly crop hundreds or thousands of our empty space. That is the context of our picture, and context has a huge importance ! For the test 34, I just added less than a thousand of context negative pictures, the result is way better. Next I increased it to note improvement, there are far less false postive. The model is still not perfect but he is quite good now.



## next steps

Ajout de feature
- Gestion des erreurs pour du ML :
    Avec l’ensemble des features ==> prendre des mesures réel pour nourrir un ML
        - Niveau sonore
        - Luminosité
        - Météo
        - Calendrier
        ( Source purement Nodesque
            - Evènement interne du Node
        )
        -packet sniffing ...

Faire de la prédiction

- Deep Learning VS Opencv


