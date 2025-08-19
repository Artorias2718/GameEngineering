# Assignment 2.11 - Finished Collision System

## Primary Requirements

- Finish your collision system. It's should:
  - Maintaining a list of all collidable objects in your game.
  - Once per frame run a collision check on all objects against all other objects (or the subset of objects it can collide with) using the swept separating axis test you implemented for Assignment 2.09.
  - For found collisions implement code to ensure objects end the frame in an uncollided state.
- Do this by moving the Physics simulation forward to the time of collision then apply velocity reflection or conservation of momentum to the colliding objects velocities, then moving the Physics simulation forward to the frame time.

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
- Must be submitted to your tags folder in a directory labeled "Assignment2.11"