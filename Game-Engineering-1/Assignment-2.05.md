---
---

# Assignment 2.05 - Data Driven Actor Creation

## Primary Requirements

- Link Lua, or some other embeddable scripting language, into your game.
- Add a function, such as Actor * Engine::CreateActor( const char * i_pScriptFilename ), that parses a script in your choosen scripting language and creates a new Actor / GameObject from it, including any related objects (Renderable, PhysicsInfo, etc.)
- Use this system to create your player and enemies.
- Remove any existing code that manually created the player or enemies.

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
- Must be submitted to your tags folder in a directory labeled "Assignment2.05"