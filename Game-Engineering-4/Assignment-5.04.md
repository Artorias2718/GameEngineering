# Assignment 5.04: Path Finding

## Requirements

- Create a square grid at least 15x15 nodes in size.
  - Randomly (or via the mouse) mark some of the nodes as "impassable" or "blocked".
  - Visualize the grid.
- Implement the A* pathfinding algorithm to find the shortest distance between two nodes.
  - Randomly (or via the mouse) select a 'start' and 'goal' node.
  - For the heuristic function, you should choose Manhattan distance, Diagonal distance (if you allow diagonal links), or straight line distance.
  - Visualize the 'start' and 'goal' nodes, the calculated path, and every node that A* put on the 'open' list.
  - Make sure you handle the case where there is NO VALID PATH.
  - What do you get when you just return a 0 from your heuristic function every time?
- [A* pseudo code](http://theory.stanford.edu/~amitp/GameProgramming/ImplementationNotes.html)
  - (And implementations in Python and C++, by the way)
- Unity reference project: Pathfinding.zip
- [Web Reference](http://www.redblobgames.com/pathfinding/a-star/introduction.html)
  - (The last interactive example on this website would meet the requirements of this assignment, by the way)