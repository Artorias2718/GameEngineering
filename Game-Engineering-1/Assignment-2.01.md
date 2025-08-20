---
---

# Assignment 2.01 - Converting a Graphical Windows based app

## Primary Requirements

- Test my Graphics Library
  - Grab my graphics library and test app from [here &#128193;:](https://code.eaemgs.utah.edu/svn/eaemgs-C06/jbarnes/dropbox/GLib)
  - Run GLibTest on any dev PCs you have to ensure it works for you.
  - Let me know if it fails for you somehow
- Convert your Monster Chase app to a Windows application
  - Create a new Visual Studio project for a Windows application. (You can do this by changing the project setting but it's a pain.)
  - Add your existing Engine project to your new solution.
  - Link in my GLib library.
  - Add the necessary GLib calls (Look through GLibTest as an example).
- Make the necessary changes to your game app to display your GameObjects as GLib textured sprites.
- Make sure your app properly shuts down when the window is closed (GLib will return a true flag to the parameter passed to GLib::Service( bool & b_Quit ); )
- For Semester 2 you can create a new 2D game of your choosing so feel free to remove any existing game specific code. You don't need AI to satisfy this assignment.

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
- Must be submitted to your tags folder in a directory labeled "Assignment2.01"