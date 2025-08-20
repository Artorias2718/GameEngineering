---
---

# Assignment 5.11: Genetic Algorithms

## Requirements

- Create a "Tanks collect the mines" game. (Minesweeper... no not that one)
  - The game is played by 15-20 AI players.  All players should all start at the same location and orientation on every "round".
  - Each player must control the tank using independent 'left tread' and 'right tread' controls.  So to move forward, the player must press both left and right treads forward simultaneously.  Pressing only left tread forward should send the tank into a clockwise circle.  Pressing both right tread forward and left tread reverse should cause the tank to spin counter-clockwise in place without moving. (See Tanks.zip Download Tanks.zip for a working demonstration)
  - Randomly populate the world with mines (30-50).  After app initialization, the position of the mines should remain constant with every round.  When a tank touches a mine, it is "collected" for that tank only, but NOT removed from the map so other tanks can also collect it.
  - The map should wrap at the edges.  If the tank goes off the left side of the screen, it should reappear on the right side.
  - Each round lasts 30 seconds. Each player tries to collect as many mines as possible within the time limit.
  - Provide HUD elements for each tank's current score, the time remaining, and a 'round over' screen showing the score the players achieved.
- Implement a genetic algorithm to improve the tank AI
  - The AI cannot **see** or otherwise detect the mines.
  - The AI only has a sequence of **left/right tread at x power** commands, which it blindly follows. Each command should be executed every 250ms at most.  (30 sec / 250ms = 120 commands minimum)

- Example Chromosome

  | Left    |  Right  |
  |---------| ------- |
  |  0.38   |  0.94   |
  |  0.72   |  -0.29  |
  |  -0.11  |  0.62   |
  |  ...    | ...     |

  - When the game initially starts, randomize the sequence of commands for each AI.
  - After each round, run the genetic algorithm on the population,
  - Implement a fitness function that evaluates a score for the number of mines collected.
- Genetic Algorithm Links:
- [Overview](http://geneticalgorithms.ai-depot.com/Tutorial/Overview.html)
- [More info](http://www.obitko.com/tutorials/genetic-algorithms/)
  - (Best applet from that site: http://www.obitko.com/tutorials/genetic-algorithms/example-function-minimum.phpLinks to an external site.)
- Genetic Algorithm tips:
  - Make all the weights have a 0.0 to 1.0 range.  This will make selecting a mutation value easier.
  - Keep the number of elite chromosomes to no more than 10% of your population.
  - Mutation chance should be very low, no more than 5%.  Most of the improvement comes from crossover alone.
- Other Fun Genetic Algorithm links:
- [Smart Rockets](http://www.blprnt.com/smartrockets/)
- [BoxCar 2D](http://boxcar2d.com/)
- [Genetic Cars 2](http://gencar.co/)