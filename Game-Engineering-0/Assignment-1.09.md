---
---

# Assignment 1.09 - Memory Leaks

## Primary Requirements

- Add a call to _CrtDumpMemoryLeaks() as the last call made before exiting main() in your MonsterChase app.
- Make sure it, and any needed header files, are wrapped in compiler preprocessor macros so it's only included in a Debug build.
- Do whatever you need to do to ensure it reports no memory leaks when your application exits.

## Secondary Requirements

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
- Must be submitted to your tags folder in a directory labeled "Assignment1.09"