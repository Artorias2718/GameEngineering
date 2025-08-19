# Assignment 5.05: Path Following

## Requirements

- Create a square grid at least 20x20 nodes in size.
  - Randomly (or via the mouse) mark some of the nodes as "blocked".
  - Allow each unblocked grid node to have a variable cost, randomly assigned (or via the mouse), at least 3 variations.  For example, if each grid node represented terrain: road tiles cost 1, forest tiles cost 2, swamp tiles cost 3.
  - Visualize the grid, including costs.
- Create many (at least 10) game entities that navigate the grid in real-time.
  - Each entity should choose a random unblocked goal grid node as it's destination, perform an A* pathfind to that goal, and then travel along the calculated path to reach the goal. Ensure each node's cost is accounted for.
  - Each entity should perform "seek"-like steering for the intermediate nodes along the path, and "arrival"-like steering for the goal node.
  - After each entity arrives (stops at) it's goal, it should randomly choose a new goal and keep going.
  - Each entity's velocity must be affected appropriately by the cost of the grid nodes as they travel across them.
  - Allow one entity to be "marked", to single it out for visualization.  Visualize the 'start' and 'goal' nodes, it's current calculated path, and it's steering vector.