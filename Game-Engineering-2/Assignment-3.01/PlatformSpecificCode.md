---
---

# Platform-Specific Code Example

- One of the classic programming guidelines is to not duplicate code. When dealing with multiple platforms this is still important but it becomes more difficult because sometimes it is impossible not to have two different implementations that do the same thing. Whenever possible you should try to create an API that hides the differences between platforms from users of your engine so that they only have to write code that will work everywhere, but when working on the engine itself there are two different methods we will use in this class to deal with multiple platforms.

## Small Differences

- In some cases a function will be almost identical except for a small difference between platforms. For example, different platforms may have different enum values for the same concept, or one platform may require an unsigned 32 bit ID and another a 64 bit pointer to identify something. In those cases you can author a single function but use the preprocessor to deal with the small differences.
- In our class we use property sheets to #define either EAE6320_PLATFORM_D3D or EAE6320_PLATFORM_GL (refer to the Solution Setup example if you need a reminder). In this example solution look at the function definition for FunctionWithSmallDifferences() to see how those are used with #if / #elif statements to deal with a small difference in a function that is otherwise identical for all platforms.
- Using the preprocessor like this should be considered the preferred way of dealing with platform differences when possible because it allows you to share code. There are times, however, when the differences are more substantial; even in cases where you could use the preprocessor to deal with differences doing so can become hard to read when there are two many differences (either too many lines or differences in the algorithm or local variables). If there are significant differences in how two different platforms work you should consider making two different functions for each.

## Big Differences

- There are two platform-specific files in the example program, Graphics.d3d.cpp that lives in a "Direct3D" subdirectory and Graphics.gl.cpp that lives in an "OpenGL" subdirectory. This is how we will deal with platform-specific functions in this class, although it's not the only way. Whenever you have a file that contains a platform-specific function name it with an extra ".d3d" or ".gl" extension and put it in a folder for that platform (both on disk and in the Solution Explorer).
- Look at the function FunctionWithLargeDifferences() that is defined separately in each of the platform-specific files. With no preprocessor #if / #elif statements it is easier to read (even in this small artificial example), although it makes it a bit harder to follow program logic since the function is defined in two different places. One of the goals of this class is to give you experience with deciding when to use each strategy for maximum readability and maintenance.
- Since you are now defining a single function twice you need to help Visual Studio know which version to use:
  - Right click one of the platform-specific files in Solution Explorer and choose "Properties"
    - Note that you must right-click the file itself, and not the project!
  - In the tree view to the left select Configuration Properties->General
  - Change the "Platform:" drop-down to the platform that should not use the file you selected
    - If you right-clicked Graphics.d3d.cpp you should change the platform to Win32
    - If you right-clicked Graphics.gl.cpp you should change the platform to x64
  - Change Excluded From Build to Yes
- By excluding the file from a build for a specific platform you make sure that Visual Studio uses the correct function definition. You should change the active solution platform in the example program while watching Solution Explorer to see the little red icon showing which file is excluded.