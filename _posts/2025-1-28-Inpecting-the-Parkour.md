---
layout: post
title: Inpecting the Parkour - What are we interacting with?
categories: [Project, IVRAR, Computer Science]
excerpt: For the project, we were given a locomotion course to test our implementations on.
---

For the project, we were given a locomotion [course](https://github.com/wenjietseng/VR-locomotion-parkour) to test our implementations on. You would run around a low-poly city, collecting coins and moving shapes until you did one full lap around the track. 
![](/images/parkour.png)

Coins are scattered about, and the track isn't just a straight line, so the player must have a way to alter their direction of movement. 
The track isn't completely flat, so we must make sure the locomotion won't break if we are at an incline. 
Some coins are in the air, so we need a way to reach those.
All of these must be taken into account while we develop the implementation.

![](/images/parkourinteraction.png)

There is also the selection task portion of the parkour, where a T-shaped block spawns and it must be oriented in the correct spot.
If we are having only one hand to work with, that is our main way to move around the game, how can we also have it be our main mode of object interaction? 
