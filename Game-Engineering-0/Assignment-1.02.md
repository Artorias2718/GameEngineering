---
---

# Assignment 1.02 - Monster Chase

## Objective

- The focus of this assignment is not really about the game or it's game play but to give us something we can deconstruct and compare how we do things in the commercial game development.

## Primary Requirements

### Game

- Submit an application for a game with a Player and some number of monsters moving about a 2 dimensional integer based grid. There will be one Player. The number of Monsters will be determined by querying the user.
- The game should have the following flow:
  1. Prompt the user for the number of Monsters.
  2. Programmatically determine the location of each Monster and give each one a unique name.
  3. Programmatically determine the starting location of the Player (can be (0,0))
  4. Prompt the user for Player movement direction or a key to quit.
  5. If the user choose to quit the game exit, otherwise:
  6. Update the position of the Player based on the users input.
  7. Use some AI (random movement is fine) to update each Monsters location.
  8. Display the position of the Monsters and the Player on a 2d or 3d grid.
  9. Go to step 4 above.
   There needs to be some game play mechanic that decreases or increases the # of Monsters during game play. For example: 1) create a new Monster, or destroy one, if two Monsters land at the same location, 2) destroy a Monster if it and the Player land at the same location.
- You can grab my sample from SVN [here &#128193;](https://code.eaemgs.utah.edu/svn/eaemgs-C06/jbarnes/dropbox/MonsterChase.exe) NOTE: It does not include the above AI, necessary for completing your assignment.

### Application

- Submit a VS2013 Solution, along with all necessary source files, that builds your game.
  - Submit all files for building the application. This include:
    - Any necessary source files (.cpp, .h, etc.)
    - Visual Studio Solution and Project files (.sln, optional .suo, .vcproj, optional .vcprojfilters)
- Your game must be 100% your own code, this includes NOT using the STL.
- SVN logs must show work in your trunk. Do not do work offline and simply drop it in your tags folder when done.

## Secondary Requirements

- Your application must compile without warnings.
- There should be no unnecessary files (.obj, .pdb, .exe, etc.) in your applications folder in your svn trunk directory or tags directory.