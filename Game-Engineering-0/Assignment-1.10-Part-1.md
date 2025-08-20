---
---

# Assignment 1.10 - Part 1 - Floating Point Numbers

## Primary Requirements

- Read Bruce Dawson's blog post on issues with floating point numbers  [here](http://www.cygnus-software.com/papers/comparingfloats/comparingfloats.htm).
- Add a IsNaN() functions for floating point types to your engine.
- Add a comparison function for floating point data types.
- Find places in your code where you can use your IsNaN() and comparison function.
- Write a unit test for your comparison function to ensure it works for the range of floating point numbers used in your Monster Chase game.

## Secondary Requirements

- Ensure your game exits without memory leaks.
- Follow the requirements / suggestions we talked about in class:
  - Think about the helpful rules on creating classes and inheritance we discussed in class.
  - Use header guardbands in all your header files.
  - Be const correct.
  - Pass by reference and return by value (where appropriate).
  - Use initializer lists in your constructors.
  - Properly inline smaller methods and operators (say less than 10 lines).
  - Try to write code readable by others.
- Submit all files for building the application. This include:
  - Any necessary source files (.cpp, .h, etc.)
  - Visual Studio Solution and Project files (.sln, optional .suo, .vcproj, optional .vcprojfilters)
- Your application must compile without warnings.
- There should be no unnecessary files (.obj, .pdb, .exe, etc.) in your applications folder in your SVN trunk directory or tags directory.
- Must be submitted to your tags folder in a directory labeled "Assignment1.10.01"