---
---

# Assignment 4.07: Third-Person Camera

## Requirements

- Visualize the Player - This can be a "snowman" using debug spheres, or something more complex if desired.
  - Visualize the player's facing direction.  This can be as simple as a debug line pointing out from the center.
- Implement a Third person follow camera
  - Keep the player in the view. (But the player does not necessarily need to stay in the center of the view.)
  - The position of the camera should be "lazy", only moving when it needs to get in "range" of the player.
  - Keep the ability to switch to your debug **fly** camera, you'll want that.
  - *optional* - Add ability to take temporary control of the camera (**left**/**right**) - +1 point extra credit
- Make the player's input control view-relative (like Super Mario 64)
  - Holding the **forward** button makes the player move away from the camera, regardless of which way the player is facing beforehand. Same thing for 
  $\leq ft$ (the player should move to the **left** relative to the camera), **right**, and **back** (the player should move toward the camera).
  - Holding the **right** or **left** button makes the player move in circles around the camera.