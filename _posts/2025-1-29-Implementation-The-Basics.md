---
layout: post
title: Implementation - The Basics
categories: [Project, IVRAR, Computer Science]
excerpt: After looking through the course, we can start getting to work on the basic foundations of our system. 
---

After looking through the course, we can start getting to work on the basic foundations of our system. 
I first started by creating a duplicate of the right hand that was included in the OVRCameraRig. This would be our giant moving hand that would stomp around the city.
I then made sure to disable the left hand and controllers, so that the player is limited to only using their actual hand. I made sure to disable updates to the root of this new hand, so it would be stuck in place but still mimic the players real hand movements.

Since I want the camera to be following this hand as it is moving around the course, I first tried making the camera rig a child of the hand, but as I tested manually moving the hand around in the editor, the camera movement was way too stiff. 
It was almost making me sick, so I disconnected the camera from the hand, and decided on moving the camera with a script, so its more smooth and adjustable. 

First I would create a offset position behind the hand that the camera would be moving to
```cs
Vector3 offset = -correctedForward * distance + correctedUp * height;
_targetPosition = hand.position + offset;

transform.position = Vector3.Lerp(transform.position, _targetPosition, followSpeed * Time.deltaTime);
```

Then I would get the rotation needed to look at the hand, and smoothly nudge the camera back to looking at it
```cs
Vector3 directionToWrist = wrist.position - transform.position;
_targetRotation = Quaternion.LookRotation(directionToWrist, correctedUp);

transform.rotation = Quaternion.Slerp(transform.rotation, _targetRotation, rotationSpeed * Time.deltaTime);
```

![](/images/HandRotation.gif)
