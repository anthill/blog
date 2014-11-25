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

We are working in a coworking space. It's a place we feel home and it's better than having our own offices as we constantly meet new people. Besides, a lot of our friends whom we like to share the space with, are doing things that have nothing to do with our geeky stuff.

The space, located in the historic center of Bordeaux, is a collaborative space, meaning everybody helps. 

There are many reasons why we wanted to measure the number of people working:

- be able to give a feedback to our sponsors (the city mayor)
- know when the space is opened (not everybody can open the offices and a lot of people would come earlier or on week ends if they knew it was possible)
- understand what are the main factors pushing people to come and work and to predict peaks of affluence.


**So we decided to measure, in real-time and with privacy in mind, how many people are working in the space.**

It is starting to work nicely, and will keep you posted.

## Network footprint

The first thing we measure is the **invisible footprint we leave around us with our connected devices**. When you turn on you computer or when your smart phone is looking for a network, "we emit" packets of data in the air. We first tried to listen to the packets passing by but we found a much more simple technique based on the local network.

A common linux command `arp-scan` is able to ask the network the mac address of all the devices that are currently connected. Try it yourself in the terminal by typing:

```
sudo arp-scan --retry=8 --ignoredups -I wlan0 10.0.0.0/24
```

(`wlan0` and `10.0.0.0` can be something else, use `ifconfig` to see what interface to choose). On my mac:

```
alexandre:~|⇒  sudo arp-scan --retry=8 --ignoredups -I en0 192.168.1.0/24
Interface: en0, datalink type: EN10MB (Ethernet)
Starting arp-scan 1.9 with 256 hosts (http://www.nta-monitor.com/tools/arp-scan/)
192.168.1.4     20:7d:74:.......   Apple
192.168.1.3     28:92:4a:.......   Hewlett Packard
192.168.1.24    00:15:af:.......   AzureWave Technologies, Inc.
192.168.1.254   f4:ca:e5:.......   FREEBOX SA
192.168.1.19    3c:77:e6:.......   Hon Hai Precision Ind. Co.,Ltd.
192.168.1.50    28:cf:da:.......   Apple, Inc.
```

So with a little raspberry pi (which is a small computer on a chip, bought for 30 €) and a wifi dongle, we're able to count the devices and even know their types.

This first measurement gives intersting results but fails when you're not connected, when your phone is idle or because you wear many connected devices.


## Computer vision

As human, we count people by looking at them. What if we could teach our small computer to do the same. The computer, given a picture of the scene, should be able to detect human faces and count them. The exercise is not so easy: you and I have been seing human faces and chairs for years and we know how to make a difference. The computer has to catch up this learning process in a few hours. 

You must feed him with examples: this is what we call the learning database. One one side, some postive pictures of humain (crop of upper body). One the other side, some negative pictures that can be anything except a human (pieces of walls, chairs, bags on the floor etc).

Once the database is created (by hand mostly), we use a learning algorithm (in this case Haar-cascades). Training a model can be easy, make the computer learn what we want can be strenuous. It requires skills and patience to chose the correct parameters. Here is a benchmarck of model I tested:

<iframe width="800" height="600" frameborder="0" seamless="seamless" scrolling="no" src="https://plot.ly/~babou/62.embed?width=800&height=600"></iframe>

If the box is below zero, the model finds more humans as it should, if it is above zero, it doesn't find all the humans. 

The red line representing the mean progress of each model shows that building a model is an iterative process. The grey box of the test 27 shows a pretty nice result. But a closer look at the results (using images with green box as the cover) reveals that they where both false postive and missed people and the two phenomenon ballanced each others. 

So instead of only automatically generated negative samples (when you take few pictures with no one and you slice them randomly), we added manually croped negative images representing objects like bags, chairs, shoes... and after some more work it converged to a far better result with less false postive.

**As the image analysis is made inside the sensor (composed of a rapsberry pi and a camera) the only thing that comes out is the number of people present at that moment, which ensures privacy by design.**

The model is still not perfect and it is impossible to detect faces that overide on the image or are too small, however it is another intersting measurment.

## Next steps

Neither ot the two measurment above are perfect but they give inforamtion. The next steps on this side project is to measure other features:

- noise level: when there is an external event, it could help the two previous techniques
- brightness: the lamps automatically turn off when nobody is around them
- C02 level (suggested by @fxbodin)
- external weather
- calendar data ...

and then we could train a model to infer the exact number of people based on these inputs...

more fun to come.


