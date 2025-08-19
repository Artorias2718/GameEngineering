# Assignment 5.02: Obstacle Avoidance

## Requirements

- Create a game entity that uses the **Obstacle Avoidance** steering behavior to avoid collisions.
  - The **Circle vs OBB** algorithm we discussed in class can be found [here](http://www.digitalloom.org/CodeWeaver/UofU/CircleVsOBB.cpp)
- Provide a terrain composed of circles for the entity to avoid.
  - The game entity must do a "reasonably good job" actually avoiding the obstacles.
  - The entity must "wrap" to the other side when it leaves the camera view
- Use debug lines to visualize velocity vectors, collision detector box, and steering vectors.
- Basically, make [this](http://www.red3d.com/cwr/steer/Obstacle.html)