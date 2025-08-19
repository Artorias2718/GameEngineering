# Assignment 2.04 - Using Strings Optimally

## Primary Requirements

- Implement a string pooling class
  - Should support adding strings to the pool while maintaining a single copy of every unique string, always returning the same pointer for the same string.
    - When adding a string, if the string is already in the pool it should return a pointer to it.
    - If the string doesn't exist in the pool it should add it.
    - All subsequent add()s off that string should return the same pointer.
  - Should support looking for a string without adding it, returning NULL if it isn't currently in the pool.
  - Should allow the user to create pools of any byte size.
  - Should support the pool filling up gracefully.
  - Find places in your current game that it may be useful. If you can't find any write a unit test for it.
- Implement a hashed string class - a class that allows you to use the hash of a string as an identifier.

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
Must be submitted to your tags folder in a directory labeled "Assignment2.04"