# Assignment1.06.02 - GameObject / GameObjectController / Interfaces

## Primary Requirements

- You should already have a class to describe an object in your engine (GameObject, Entity, Actor, etc.). If you don't rearchitect so you do.
  - This object should, at a minimum describe the objects position.
- Add a GameObjectController (name choice is up to you) interface class, as we discussed in class.
  - This should be an abstract class outlining the interface for controlling a game object.
- Create 3 actual game object controller classes derived from this interface class.
  - One should be a user input controller (i.e. Player) that controls the game object from keyboard input.
  - One should be a monster controller that controls the game object as a monster in your game.
  - One of your choice. Some ideas: wander randomly, go toward another game object.

NOTE: I rushed through the GameObject / IGameObjectController stuff in class. We'll talk a little more about it next week if you're still a little (or a lot) confused.

## Secondary Requirements

- There should be no unnecessary files (.obj, .pdb, .exe, etc.) in your applications folder in your SVN trunk directory or tags directory.
- Make sure your code compiles without warnings.
- Must be copied from your trunk to your tags folder in a directory labeled "Assignment1.06.02"