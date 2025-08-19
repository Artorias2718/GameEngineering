# Assignment 2.13 - Save / Load System (Serialization)

## Primary Requirements

- Add a Save / Load System to your Game and Engine.
  - When the user presses the 'S' key it should:
    - Serialize the Game Objects (and Controllers if necessary) to a buffer in a binary format.
    - Save the buffer to a file. You may use a preset filename.
  - When the user presses the 'L' key it should:
    - Load the save file to a buffer in memory.
    - Deserialize the save file, resetting the Game Objects (and Controllers) to their state at the time the game was saved.
- You should be able to:
  1. Load your game and move objects around.
  2. Create a save game by pressing 'S'.
  3. Restart the game.
  4. Load the save game by pressing 'L'.
  5. At this point the Game Objects (and Controllers) should be back in the state (location, orientation, etc.) they were at the time the save game was created.

## Secondary Requirements

- Ensure your game exits without memory or DirectX Object leaks.
- Follow the requirements / suggestions we talked about in class:
  - Think about the helpful rules on creating classes and inheritance we discussed in class.
  - Use header guardbands in all your header files.
  - Be const correct.
  - Pass by reference and return by value (where appropriate).
  - Use initializer lists in your constructors.
  - Properly inline smaller methods and operators (say less than 10 lines).Try to write code readable by others.
- Submit all files for building the application. This include:
  - Any necessary source files (.cpp, .h, etc.)
  - Visual Studio Solution and Project files (.sln, optional .suo, .vcproj, optional .vcprojfilters)
- Your application must compile without warnings.
- There should be no unnecessary files (.obj, .pdb, .exe, etc.) in your applications folder in your SVN trunk directory or tags directory.
- Must be submitted to your tags folder in a directory labeled "Assignment2.13"