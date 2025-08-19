# Assignment 2.02 - Running in real time

## Primary Requirements

- Convert you game to run a non-blocking main loop.
  - Your main loop must not wait for user input.
- Add a Timing system to your engine that uses the platforms high resolution tick counter to accurately time events.
  - Use it determine the time elapsed each time your main loop or how long between Physics updates.
- Add a Physics system that uses a Numerical Integration method to update your Game Objects as we discussed in this weeks class.
  - You may use a variant of Euler, Verlet or Runge-Katta
- One of your Game Objects needs to me driven by a user keypress.

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
- Must be submitted to your tags folder in a directory labeled "Assignment2.02"