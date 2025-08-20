---
---

# Assignment 1.08 - Block Based Heap Allocator - Part 3

## Primary Requirements

- Add a garbage collection function to your Heap Allocator.
  - It's job is to walk the list of descriptors of free memory blocks and see if it can collapse blocks that adjoin into larger blocks.
  - When it collapses two blocks it should return one of the block descriptors to the free descriptor list.
  - It should continue to do this until all possible collapses have happened and only non-collapseable blocks are left.
  - I put together a small piece of [sample code &#128193;](https://code.eaemgs.utah.edu/svn/eaemgs-C06/jbarnes/dropbox/EAE6300/Lecture10/Collect.cpp). It has a sample of how you might check two blocks and collapse them.
  - Collect.cpp also contains a sample unit test. Your HeapManager should pass that or a similar unit test.

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