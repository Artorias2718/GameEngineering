---
---

# Assignment 1.03 - Game/Engine Split

## Objective

- Convert your Monster Chase VS Solution and code base to use separate Game and Engine VS projects and directories, as we discussed in class.

## Primary Requirements Part 1 - Visual Studio Setup for separate Game and Engine projects

- Create a new (or modify your existing) Monster Chase game VS Solution so it uses two VS Projects, one for your Engine, which creates a static library from your Engine code and one for the game itself, which links in the Engine library and creates your Game application.
- Populate your two new VS projects with your existing code, putting Engine code in the Engine Project and Game code in the Game project. Engine and Game code must also reside in separate directories.
- Also:
  - Ensure the VS Game project is dependent on the Engine project so they build in the proper order.
  - Ensure the Game projects  **Additional Include Directories**  setting properly includes a path to your Engine code tree using a relative path.
  - Ensure the Game projects  **Additional Library Directories**  setting properly includes a relative path to the location where VS generates your Engine library.
  - Ensure your Engine library files is included in the Game projects  **Additional Dependencies**.
  - Ensure the above settings work for all VS build configurations. Use VS Macros where applicable.

## Primary Requirement #2 - A 2D (or 3D) Coordinate Class

- Ensure your Engine includes a class to represent a 2D (or 3D) location in space.
- Use this class, at a minimum, to represent the position of Monsters and the Player in your game. You'll probably find other uses for it.
- Include any methods you feel are necessary or valuable in the class.

## Secondary Requirements

- Your Visual Studio Solution must not use any absolute paths (i.e C:\Users\Joe....)
- Submit all files for building the application. This include:
  - Any necessary source files (.cpp, .h, etc.)
  - Visual Studio Solution and Project files (.sln, optional .suo, .vcproj, optional .vcprojfilters)
- SVN logs must show work in your trunk. Do not do work offline and simply drop it in your tags folder when done.
- Your application must compile without warnings.
- There should be no unnecessary files (.obj, .pdb, .exe, etc.) in your applications folder in your SVN trunk directory or tags directory.
- Must be submitted to your tags folder in a directory labeled "Assignment1.03"