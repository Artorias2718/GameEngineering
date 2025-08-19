# Assignment 2.06 - Multi-Threaded Data Processing

## Primary Requirements

- If needed spend some time analyzing my multi-threaded file processing sample. It can be found in my SVN repository [here:&#128193;](https://code.eaemgs.utah.edu/svn/eaemgs-C06/jbarnes/dropbox/EAE6310/Lecture%2006/Threading).
  - Please take the time to look it over before next Tuesdays class so we can go over any questions you have.
- Modify your engine so it loads and processes lua files to create game actors (from Assignment 2.05) in threads so it doesn't block your game loop.
  - You should have one thread that handles asynchronous File I/O driven by requests from the main thread.
  - You should have another thread that processes files.
  - Create whatever classes you need to support threads and thread synchronization objects (mutexes, semaphores, etc.)
  - Feel free to use any code you want from my Threading sample.

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
- Must be submitted to your tags folder in a directory labeled "Assignment2.06"