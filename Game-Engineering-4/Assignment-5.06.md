# Assignment 5.06: State Machines

## Requirements

- Create a square grid at least 20x20 nodes in size. (Same as previous assignment)
  - Randomly (or via the mouse) mark some of the nodes as "blocked".
  - Allow each grid node to have a variable cost, randomly assigned (or via the mouse), at least 3 variations. For example, if each grid node represented terrain: road tiles cost 1, forest tiles cost 2, swamp tiles cost 3.
  - Visualize the grid, including costs.
- Add the following dynamic game objects to the map via the mouse:
  - Keys: red, blue, and green key pickups.
    - Each key is always available to be picked up... meaning the AI could **accidentally** pick it up on it's way to another target.
    - On pickup, it should be removed from the map, and added to the AI's (virtual) inventory.
  - Doors: red, blue, and green doors.
    - The door should cause the grid node it lies upon to be blocked unless the AI holds the correctly colored key.
    - The door must visually change to an "open" state after the AI reaches the door while holding the key.
  - Treasure: A final pickup (but mostly just a goal).
    - The treasure can only be obtained after all 3 doors have been opened with keys.
- Create a single game entity with an AI that performs it's logic via a Finite State Machine (FSM)
  - The FSM should attempt to collect the treasure. (Collect keys -> Open doors -> Collect Treasure)
  - The entity pathfinding should perform "seek"-like steering for the intermediate nodes along the path, and **arrival**-like steering for the goal node.
  - The current FSM state name must be displayed on the screen.
- Reference: Stubbed out FSM code and Image files - StateMachine.zip