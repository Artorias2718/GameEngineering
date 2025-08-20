---
---

# Assignment 1.11 - Controlling Memory Allocation

## Primary Requirements

- Write a FixedSizeAllocator class, such as we've discussed in class over the last two weeks.
- Overload any new() and delete() operators used by your MonsterChase game so that they use your HeapAllocator and FixedSizeAllocator.
- Test to see what small allocations (say under 128 bytes) your game makes. Ensure these go through FixedSizeAllocators.
- Make sure your HeapAllocator and FixedSizeAllocator can detect outstanding allocations on shutdown. Use this to detect memory leaks when your game shuts down and display the appropriate information in VS Output window.

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
- Must be submitted to your tags folder in a directory labeled "Assignment1.11"