---
---

# Assignment 4.04: Player Collision

## Requirements

- Your Maya plugin must export collision data from the 'ctf_map' file.
  - The exported data should be a custom format that your engine understands.
  - Hint: You probably don't want triangle strips, but instead just a flat list of triangles.
- Make a 'Player' entity that collides with the ground and walls.  The camera is attached to this player's transform, like a 1st person game.
  - The player does not fly, it should simulate gravity.
  - The player must be able to move forward and backward within the constraints of the collision. (It should not walk through walls, etc.)
  - The player must be able to go up and down inclines like the stairs, which is just a ramp.
  - The player must not get "stuck" on walls.  Instead the player should "slide" along a wall when being pushed into it.
  - Implementation:  Pick a 'height' for your player.  Shoot a ray from from this position straight down to determine where the ground is. (Intersection code attached)
  - It is okay if your player falls out of the world sometimes... it happens.  But it should not happen often!
  - This implementation will probably make your frame rate drop.  Don't worry about it... we will address that in a future assignment.
- Switching from the Player to the Fly Cam should leave the Player behind.  When switching back, the camera should reattach itself back to the position and orientation it was in before the Fly Cam was enabled.
  - Idea: You may want to draw a debug shape at the position of the Player when the Fly Cam is active. (only)
-  The **right** way to apply velocity and acceleration over time:
    ```cpp
    - position += velocity * delta_time + 0.5 * acceleration * delta_time
    - velocity += acceleration * delta_time
    ```
- The intersection code we went over in class: [Intersection.cpp](./Intersection.md)
- (Feel free to use your own if you wish.)