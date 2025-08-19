# Assignment 2.12 - Profiling & Optimization

## Primary Requirements

- Grab my collision demo from [here](https://code.eaemgs.utah.edu/svn/eaemgs-C06/jbarnes/dropbox/EAE6310/Lecture%2012/CollisionDemo) (Links to an external site.)
- Profile it using Visual Studio's built in profiling tools.
- Analyze the profiling results to find places where the math may be optimized using SIMD vector instructions.
- Look through the SIMD Vector class from [this](https://code.eaemgs.utah.edu/svn/eaemgs-C06/jbarnes/dropbox/EAE6310/Lecture%2012/HWVector) (Links to an external site.) example.
- Optimize the collision demo using SIMD vector code.
- Profile the collision demo afterward to see if your optimizations made a positive difference.

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
- Must be submitted to your tags folder in a directory labeled "Assignment2.12"