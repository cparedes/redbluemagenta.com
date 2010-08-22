--- 
wordpress_id: "447"
layout: post
title: Breakout from scratch, part I
wordpress_url: http://blog.redbluemagenta.com/?p=232
---
[caption id="attachment_233" align="aligncenter" width="300" caption="Breakout, the game"]<img class="size-medium wp-image-233" title="Breakout Game" src="http://blog.redbluemagenta.com/wp-content/uploads/breakout-300x234.jpg" alt="Breakout, the game" width="300" height="234" />[/caption]

So, I've been trying to figure out how to code the game "Breakout" from scratch as an exercise in game development.  One of the things that I've been trying to flesh out is how to model the behavior of the ball as it hits the different parts of the paddle.  For one, the ball should change it's angle so that it travels more to the left (if it hits the left side of the paddle) or more to the right (if it hits the right side).  Second, if it hits the "dead zone" in the center, then the angle should be preserved (so the "dead zone" will be a few pixels wide, so that the player doesn't have to be super precise when he wants the ball to travel with the same direction.)  Third, if we are representing the ball's motion with a vector, then the "x-axis" vector (I put "x-axis" in quotes for a reason, which will be explained in a little bit) should not change, while the "y-axis" vector will be flipped in direction (so if the ball was traveling down and left, then the ball will still travel left, but will now be traveling up instead.)

Using a bit of geometry and trig, we want the y-axis vector to be multiplied by -1 so that the orientation is flipped (analogous to reflecting the ball's motion vector over the y-axis, and drawing the arrow away from the origin, rather than toward the origin):

[caption id="attachment_235" align="aligncenter" width="300" caption="Simple reflection"]<img class="size-medium wp-image-235" title="Diagram 1" src="http://blog.redbluemagenta.com/wp-content/uploads/dia1-300x112.gif" alt="Simple reflection" width="300" height="112" />[/caption]

But what should the ball's behavior be when it hits the paddle to the left or to the right of the dead zone?  We want the y-axis vector to flip AND reduce in magnitude, plus we want the x-axis vector to proportionally increase in magnitude:

[caption id="attachment_236" align="aligncenter" width="300" caption="Hitting the paddle&#39;s edge"]<img class="size-medium wp-image-236" title="Hitting Paddle Edge" src="http://blog.redbluemagenta.com/wp-content/uploads/dia2-300x112.gif" alt="Hitting the paddle's edge" width="300" height="112" />[/caption]

So I figured that we could do this fairly easily if we imagined drawing a "collision plane" and it's corresponding normal vector, so that instead of reflecting over an absolute y-axis, we reflect over the plane's normal vector:

[caption id="attachment_237" align="aligncenter" width="300" caption="Collision plane reflection"]<img class="size-medium wp-image-237" title="Collision Plane" src="http://blog.redbluemagenta.com/wp-content/uploads/dia3-300x112.gif" alt="Collision plane reflection" width="300" height="112" />[/caption]

The math is probably messier than keeping everything in terms of the xy-plane (I haven't worked it out quite yet), but it would generalize nicely when we consider all possible collisions along the paddle.

So, here's what I imagine how some of the collision planes would look like when we are considering the possible ways that the ball could hit the paddle:

[caption id="attachment_238" align="aligncenter" width="300" caption="Hey look, collision planes"]<img class="size-medium wp-image-238" title="Collision Plane Enumeration" src="http://blog.redbluemagenta.com/wp-content/uploads/dia4-300x112.gif" alt="Hey look, collision planes" width="300" height="112" />[/caption]

So the corners of the paddle would have a collision plane tilted about 45 degrees in relation to the xy-plane, and the angle it makes with the xy-plane scales down toward 0 degrees when we move closer toward the dead zone.

However, I think there's still a few problems with this model.  For one, I need to play the game a bit more in order to get the behavior right. :p  Second, there could be problems with the ball bouncing totally parallel to the line that it travels on, even if the ball's motion vector is going toward the edge of the paddle (I don't know if this is really a problem, though.)

I think next time, I'll try to talk about how to go about modeling more of the possible collisions and modeling some of the game logic.
