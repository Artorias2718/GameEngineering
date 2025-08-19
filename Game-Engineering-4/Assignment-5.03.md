# Assignment 5.03: Flocking

## Requirements

- Create at least 20 AI entities that exhibit the "flocking" steering behavior.
  - The AI Entities must have some visual indication of their facing direction.
  - Ensure the three components of flocking are easy to identify: Separation, Cohesion, and Alignment.
  - Just for fun, see how many flocking entities you can put into your system before the frame rate drops to about 10-15. Does your flocking still "work" with that many entities?
- Use debug lines to visualize your steering vectors.  But provide a control to turn them off.
  - You are NOT required to visualize your velocity vectors on this assignment.
- Provide debug controls to adjust the weights on Separation, Cohesion and Alignment.
  - *optional* - Consider adding a button to visualize the various components of flocking on a single entity (neighbor radius, separation, cohesion, etc.)
- *Extra Credit* (1pt) - Add a pursuer AI entity into the system. The flocking AI must evade the pursuer while continuing to flock.

- Unity reference project: Flocking.zip
- Reference: [Boids](http://www.red3d.com/cwr/boids/) by Craig Reynolds
- [Here](https://www.youtube.com/watch?v=M028vafB0l8) is a good example of 2D flocking with lots of extra features