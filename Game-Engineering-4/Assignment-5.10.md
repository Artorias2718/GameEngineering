# Assignment 5.10: Minimax

## Requirements

- Create a game of 6x6 tic-tac-toe, with each line of 4 X's or O's scoring a point until every space is filled.
  - Example: http://www.atksolutions.com/games/tictactoedeluxe.htmlLinks to an external site., Select "6 x 6 Draw 1 - Most 4 in a row wins"
  - Display current score, and a simple win/lose condition indicator.
- Implement an AI opponent.
  - Use a MiniMax game tree of at least depth 5.
  - Display board evaluation score for chosen move.
- No video recordings, only interactive submissions will be accepted.
- References:
- [MiniMax pseudocode](https://chessprogramming.wikispaces.com/Minimax)
- [Alternate Negamax implementation](https://chessprogramming.wikispaces.com/Negamax)
- ... and a [visualization with chess](http://www.youtube.com/watch?v=Xb5KkyKiz8g)
- [With Alpha-Beta pruning](https://chessprogramming.wikispaces.com/Alpha-Beta)
- ... and [another](http://www.ocf.berkeley.edu/~yosenl/extras/alphabeta/alphabeta.html)
- Remember the simplest board evaluation function for this Tic-Tac-Toe game can simply be calculating the score for both sides, then subtracting the Min player's score from the Max player's score.  (AI Player Score - Human Player Score)