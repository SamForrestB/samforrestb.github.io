---
layout: post
title: Thinking About Hands - What would be a cool VR interaction?
categories: [Project, IVRAR, Computer Science]
excerpt: In VR, we can do a lot of things that you can't do in real life.
---

In VR, we can do a lot of things that you can't do in real life.

You can fly, teleport, pilot giant machines, transform into different forms, etc etc etc.

Of course, many of these things have been done over and over, and with the IARVR project, we need to come up with something unique that isn't just the first thing that comes to mind.

So the first thing I started doing was thinking, 'what is something I don't see in VR games that I would want to experiment with?' What I thought of first was recording movement and gestures, and then assigning them to inputs that would replay those actions.
I imagined a small control room where to your left you would record yourself, lets say grabbing an item, and then you would take a floppy disk of that action and slide in into a main control pannel. This pannel would have multiple buttons, switches, levers, and slots so the player could choose what goes where.
Outside of this control room, we would see the robot that all these recorded movements are affecting, and that would be the game. Piloting this robot through replays and tank controls. You would move the legs with two levers like Plankton in Spongebob, you could rotate the body with a big handwheel, but anything with its arms would need to be recorded.

I Liked the idea, but then I started going through everything I would need to implement it. Recording the player, saving those movements, playing BACK those recordings on a different model. I tried messing around with the Unity Animation Recorder, but I was having trouble translating the movements of my hands from the animation file to a kinematic humanoid. If I spent more time playing around with it, i'm sure I could get something to work, but I wanted to experiment more.

My next idea came to me while I was messing around with the no-hands mode on the Quest 3. What if you could pilot your hand like Thing from the Addams Family? Have it skitter and crawl around an environment?
![](/images/thething.gif)
I would have a seperate ridgidbody hand that mimics exactly what your hand is doing, and what I wanted was for it to be moved through pure physics. It would be a challenge to get it moving and steady, and take a good amount of work to master.
But it turns out that the Meta XR All-in-one hands do NOT like being their own ridgidbodies, so while yes I could make a copy of my hand and move its fingers, the hand wouldn't interact with its environment. Once again I think if I experiment with it more I may be able to make a breakthrough, but I thought of something that could build upon this hand idea.

So if I couldn't get the hand to move by itself, I thought the next best thing would be getting it to move indirectly. What if I attach the hand to an invisible ball, and then use pose detection and hand position to move it? When I make a 'running' pose with my hands, it begins moving forward, and turning my hand to the left or right would rotate it.
This could be a way to get something to actually work, and then I could iterate on it more and more, adding small elements here and there.
Either way, its better than having nothing to show!
