# Assignment 2.03 - Smart Pointers

## Primary Requirements

- Implement a templated reference counting smart pointer.
  - Should provide accurate reference counting of active smart pointers to the shared underlying object, releasing the object when reference count reaches 0.
  - Should provide natural semantic access to the underlying pointer without providing direct access to the pointer.
  - Should provide enough overloaded comparison operators( ==, !=, !, etc.) for the underlying pointer for natural use.
  - Should support NULL as a value for the underlying pointer.
  - Should support default creation (i.e. unassigned smart pointers).
- Find use cases for the smart pointer class in your Game and/or Engine.
- If you can't find any use cases provide a unit test.
- Make it awesome.

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
- Must be submitted to your tags folder in a directory labeled "Assignment2.03"