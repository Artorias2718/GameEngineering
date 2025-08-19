# Assignment 4.10: Capture The Flag

## Requirements

- Mark the game objects required for CTF gameplay using your Maya plugin
  - Flag - When the player reaches a reasonable distance away from the flag, they begin to "carry" it.  Meaning the renderable geometry of the flag will literally move with the player's character. NOTE: The player should not be able to carry their own team's flag, only the flag of the opposing team.
  - Score Zone - If the player enters the score zone on their team's side of the map, while carrying the flag:
    - The player's team score is increased.
    - Both flags are reset back to their original locations.
- Tag - Who needs guns? :)
  - To stop the flag carrier from returning to his goal, the player must "tag" him. (Meaning: the distance between the two players should be less than some minimum value)
  - If a player is "tagged" while carrying a flag, then the flag is reset back to it's original position.
  - If both players are carrying a flag when the "tag" occurs, then both flags are reset.
  - *optional* +1 extra credit: Instead, use guns with slow projectiles (like a rocket)
- Display both team's scores onscreen.
- Sprint/Stamina meter
  - Holding down a button will cause the player to run 2-3 times as fast as normal, but drains the stamina meter.  When the stamina meter is empty, the player can no longer sprint.  When the player is not holding the button, the stamina meter fills 3-5 times slower than when it drains.  The player should be able to sprint for 5-10 seconds with a completely full stamina meter.
  - Draw a visualization of this meter onscreen.
- Networking
  - The client's player character joins the opposing team.
  - The game score should be kept in sync between the two peers.
  - Any "Pause" functionality should be disabled on both server and client, but only when connected.
- Submit a video of each of these requirements in action... I don't need to see both the client and server this time, just the client will do.