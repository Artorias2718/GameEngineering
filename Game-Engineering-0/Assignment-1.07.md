# Assignment1.07 - Debug Logging / Asserts

## Primary Requirements

- Add a debug logging function, similar to what we discussed in class, to your Engine.
  - You can use my DebugLog() function as a template.
  - It must contain some functionality beyond what my function already includes. Ideas:
    - Outputting the file and line where the DebugLog() event occured.
    - Outputting the system where the DebugLog() event originated.
    - Supporting verbosity levels.
  - It must support variable arguments.
  - It must not output anything in a non-debug build.
- Add a custom assert handler to your Engine.
  - You may use my MessagingAssert code as is.
- Find possible failure points in your code and add asserts. Ideas:
  - Validating inputs to a function.
  - Validating return values from other functions / system calls.
- Find places in your code that would benefit from debug logging. Add debug logging.

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
- Must be submitted to your tags folder in a directory labeled "Assignment1.07"