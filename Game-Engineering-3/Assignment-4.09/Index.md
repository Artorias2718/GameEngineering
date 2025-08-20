---
---

# Assignment 4.09: Networking

## Requirements

- Add client/server network communication.
  - Provide a way to start your game in either client mode or (non-dedicated) server mode. The server listens, the client connects.
  - In client mode, your game should not create any additional players until the server instructs it to. (You might consider delaying the construction of the human player until this time as well)
  - Messages sent between client and server must be reliable for important "must arrive" messages, and unreliable for kinematic state updates.
  - Messages do not have to be binary. To ease debugging, you might consider using XML or JSON for their readability.
- Visualize the remote player's character on both client and server.
  - From the server side: As client connects, the client should be notified of his initial position and orientation.
  - All player's positions and orientations should be kept in sync between the two peers. 
  - *optional* +1 point if you implement client prediction.  You'll have to sit down with me and show me your code sometime.
- Submissions will only be accepted in VIDEO format, showing both client and server communicating.
- You can use third-party networking software to complete this assignment.
  - The software we discussed in class is [RakNet:](https://github.com/OculusVR/RakNet)
    - [RakNet basic message client/server tutorial](http://www.jenkinssoftware.com/raknet/manual/tutorial.html)
    - [RakNet packet creation documentation](http://www.jenkinssoftware.com/raknet/manual/creatingpackets.html)