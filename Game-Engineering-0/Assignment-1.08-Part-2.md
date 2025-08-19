# Assignment 1.08 - Block Based Heap Allocator - Part 2

## Primary Requirements - Part 1

- Enable to x64 platform for your Game.
- Ensure that your code compiles without warnings on the x64 platform.

## Primary Requirements - Part 2

- Continue your Heap Allocator by adding a free() function, that allow the user to return a previously allocated block.
- You do not need to implement garbage collection yet.
- It should validate, via asserts, that the returned address was an address actually provided to the user via the alloc() function.
- Use the correct C++ style casts in your Heap Allocator.
- Use the correct data type so your code functions correctly on the x64 platform.
- Your game does not need to run on the x64 platform yet.

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
- Merge the changes for Assignment 1.08 Part 2 into the existing tags folder for Assignment1.08