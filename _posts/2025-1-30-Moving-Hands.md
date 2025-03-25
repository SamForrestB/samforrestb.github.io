---
layout: post
title: Implementation - Moving Hands
categories: [Project, IVRAR, Computer Science]
excerpt: With the camera following our hand, and the player able to rotate, its time to actually move.
---

With the camera following our hand, and the player able to rotate, its time to actually move.
I first started by trying to use purely the force of the players actual hand to move around, meaning that the player would need to push their fingers against the ground to gain tracktion and move forward.
The hands already came with all the capsule colliders needed to give collision to the hand, and when first testing, you could slightly move forward by flopping around.
But because we have to lock the root of the hand so that it doesn't teleport back to where the players real hand is, the hand can't really pose itself in any manner that would assist in moving it.
The palm is basically locked, but if we wanted to actually make some movement it would need to be able to move freely, which I sadly wasn't able to get to work.

What I had to do instead was just create a box collider that is roughly the size of the hand, and we would just move that around the track.
I still wanted the player to move their fingers in order to move forward, so I used the same idea that I used for detecting the head rotating: Make a trigger collider where when a finger enters it, the hand moves forward.
I added new tags to the capsule colliders on each fingertip, and then begun working on the scripting for the movement. This would be another new method in the locomotion technique script.

First we would keep track of what fingers have entered the collider, and what time that they entered:
```cs
 HashSet<Collider> activeFingers = new HashSet<Collider>();
 foreach (Collider collider in colliders)
 {
     if (collider.CompareTag("FingerTip"))
     {
         activeFingers.Add(collider);

         // Record the entry time if the finger is new
         if (!fingerEntryTimes.ContainsKey(collider))
         {
             fingerEntryTimes[collider] = Time.time;
         }
     }
 }
```
This would be so that we can compare the time the finger entered with the set time that the finger is allowed to keep accelerating the hand:
```cs
float targetSpeed = 0;
foreach (var finger in fingerEntryTimes.Keys)
{
    if (Time.time - fingerEntryTimes[finger] <= fingerAccelerationTime)
    {
        targetSpeed += maxSpeed / fingerDetectionThreshold;
    }
}
targetSpeed = Mathf.Clamp(targetSpeed, 0, maxSpeed);
```
This time limit that each finger has to accelerate the hand was added because the player could just hold their hand in a fist and move around the map, which was not what I intended.

![](/images/MovingFist.gif)

After the target speed is determined, we pass it into another method called MoveHandSmoothly. This would actually transform the position of the hand based on the given target speed:
```cs
 float speedDifference = targetSpeed - currentSpeed;

 if (fingerDetected) 
 {
     currentSpeed += Mathf.Sign(speedDifference) * acceleration * Time.deltaTime;
     currentSpeed = Mathf.Clamp(currentSpeed, 0, maxSpeed); 
 }
 else 
 {
     currentSpeed += Mathf.Sign(speedDifference) * deceleration * Time.deltaTime;
     currentSpeed = Mathf.Clamp(currentSpeed, 0, maxSpeed); 
 }

 transform.position += moveDirection * currentSpeed * Time.deltaTime;
```

But it would also slow the hand down when no fingers are detected:
```cs
 else if (!fingerDetected && _currentSpeed > 0)
{
    float speedDifference = targetSpeed - _currentSpeed;
    currentSpeed += Mathf.Sign(speedDifference) * deceleration * Time.deltaTime;
    currentSpeed = Mathf.Clamp(currentSpeed, 0, maxSpeed); 
    transform.position += moveDirection * currentSpeed * Time.deltaTime;
}
else if (!fingerDetected && currentSpeed == 0)
{
    moveDirection = Vector3.zero;
}
```

Here you can see the fixed movement, where the player must keep moving their fingers if they want to move fast around the course.

![](/images/FixedMovement.gif)

And here is a better look at the colliders that are used to move the hand.

![](/images/ColliderMovement.gif)

Now that the hand is able to run around the course, it also needs to be able to jump, as some of the coins at the end of the course are in the air.
What I decided to do was check when all the fingers are within the collision box, and then when the player fans out their hand, it adds upwards force to the hand.
I needed to track the time when all fingers are inside the box, and then check if they left in a certain amount of time to trigger the jump:
```cs
if (activeFingers.Count == fingerDetectionThreshold) 
{
    allFingersInTime = Time.time; 
}
else if (activeFingers.Count == 0 && allFingersInTime != -1f && Time.time - allFingersInTime <= jumpTimeThreshold)
{
    hand.GetComponent<Rigidbody>().AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
    allFingersInTime = -1f; // Reset the timer
}
else if (activeFingers.Count == 0)
{
    allFingersInTime = -1f; 
}
```

Here you can see that when all the fingers leave the collider, the hand jumps up! This can be chained one after the other, so you can actually fly around if you want!
![](/images/HandJump.gif)
