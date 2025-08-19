# Assignment 3.02 - Introduction to Lua

- Before trying to work on this assignment you should study and understand the following example program:
  - [Using Lua](./UsingLua.md)

## Requirements

- Update your asset build system to use Lua
  - Add Lua to your solution
  - Register C asset building functions with a Lua state
  - Call a Lua function to build each asset, which will in turn call the C functions
- Update the UserSettings project to actually use a user-configurable settings.ini file instead of hard-coding the game's resolution
  - Add a logging message if the settings.ini successfully sets a resolution width, and another logging message if the settings.ini successfully sets a resolution height
- Add a second triangle to the vertex buffer so that a square is drawn instead of a triangle
- Your write-up should:
  - Show a screenshot of your game running
    - It's ok to still have vertex position animation, but it should be obvious that there are now two triangles, and that you are drawing a square (possibly distorted) and not just a single triangle
- Discuss the flow of the asset build system as it currently exists
  - (Describe how a source asset ends up being a target asset, tell us what AssetBuildSystem.exe does and what AssetBuildSystem.lua does, etc.)
- Tell us about the dependencies of the changed projects:
  - What does Lua depend on? What depends on it?
  - What does LuaExe depend on? What depends on it?
  - What does AssetBuildSystem depend on? What depends on it?
  - What does UserSettings depend on?
- Describe what happens if you build the solution, change the resolution in settings.ini in $(GameDir), and then build the solution again
  - Will the settings.ini in $(GameDir) get changed back to the source file or stay as the one that you changed? Explain why.
  - Is this behavior desirable or is it unfortunate? Explain why.

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- If you delete the temp/ folder, and then build the BuildAllAssets project only, Visual Studio should report no errors and the vertex and fragment shaders should end up in the correct $(BuiltAssetDir).
  - (Note that the game itself should not be in $(GameDir) in this case)
- If you put a typo in one of the keys to the environment variables that AssetBuildSystem.lua is looking for and then build the solution you should see an error message in Visual Studio's Error List that tells what the problem is. If you double-click the error Visual Studio should open AssetBuildSystem.lua and go to the line with the call to error().
  - (For example, change line 12 to "xAuthoredAssetDir")
- If you put a syntax error in AssetBuildSystem.lua and then build the solution you should see an error message in Visual Studio's Error List that tells what the problem is. If you double-click the error Visual Studio should open AssetBuildSystem.lua and go to the area with the error.
  - (For example, type asdf on line 4)
- If you put a run-time error in AssetBuildSystem.lua and then build the solution you should see an error message in Visual Studio's Error List that tells what the problem is. If you double-click the error Visual Studio should open AssetBuildSystem.lua and go to the area with the error.
  - (For example, type ```asdf = jkl[123]``` on line 4)
- If you rename one of the source shader files that's supposed to be built and then build the solution you should see an error message in Visual Studio's Error List that tells what the problem is (that the source file doesn't exist)
- If the solution is built and you change the desired resolution in the settings.ini in $(GameDir) to 1024 x 256 and then double-click the EXE to run the window should be twice as wide and half as high, without having to build anything in Visual Studio

## Details

### Asset Build System

- Add Lua to your solution
  - The provided files Download provided fileshave everything that you need. You will just need to add the projects to your solution properly, as described in the Using Lua example. Most of the appropriate settings are saved in the project files themselves, but remember that you will have to set the project dependencies after you add them to your solution.
- Update the AssetBuildSystem project
  - I have given you the files you need Download the files you need, and so you should start by copying everything in the provided AssetBuildSystem folder into your AssetBuildSystem folder (and overwriting the existing files)
  - You will have to set up the project dependencies. What does the AssetBuildSystem need in order to build? You may want to look in the "Additional Dependencies" field of the project's Linker properties to see which libraries it uses. If any of those libraries are built by your solution then you need to make sure that they are set as project dependencies so that the build order will be correct.
  - You should take the time to look through the code (both C++ and Lua) to figure out what it's doing
    - Functionally, the project should do the same thing that it did in Assignment 01. The difference is that now the logic is contained in AssetBuildSystem.lua.
    - The way that C++ and Lua interacts in this project should be familiar if you have spent the time reading and understanding the Using Lua example. If there is something in the provided AssetBuildSystem files that you don't understand you should probably review that example, because as the class progresses I will give less and less code and you will be expected to write it on your own.
  - I have removed some functionality from AssetBuild.cpp and replaced it with EAE6320_TODO. The code won't compile until you have filled in the missing functionality.
    - Everything I have removed is in C functions called by Lua, and deals with getting input parameters from Lua and passing output parameters to Lua
      - The Using Lua example teaches you how to do this
      - Additionally, there are some functions I have left completed for you to use as reference: luaDoesFileExist() is completely functional and used by the current AssetBuildSystem.lua, and luaExecuteCommand() and luaInvalidateLastWriteTime() are both completely functional although not used yet. You can look at all three for help figuring out how to fix the broken ones.

### User Settings

- In Assignment 01, I provided a UserSettings project that looked like it provided settings that the user (i.e. the person playing your game) specified, but if you looked at the implementation you would have seen that the values were just hard-coded! Now that we have learned how to use Lua, however, I have provided an implementation that reads a Lua file to get values.
- Like with the AssetBuildSystem, you can copy the provided UserSettings folder Download provided UserSettings folderon top of your existing one from Assignment 01 and replace everything. The only thing that you should have to set will be the project's dependencies.
- Take note of the settings.ini file that is part of the project. It isn't used to build the project, but if you right-click the file itself and choose Properties then you can see how it has a custom build tool set up for it. This is the same technique used for copying the Lua license (described in the Using Lua example).
  - This file provides the user a way to configure things about the game. Most users will want to change settings from an in-game GUI, but there are various reasons why an external file (that the GUI saves its changes) to is nice. In our class we won't take much advantage of this, but it is one other way we can attempt to make things easier to work with and more data-driven (less hard-coded). You can think of this file as a sort of asset that the user can control.
  - This also shows another reason we use Lua in this class: It provides a really powerful and quick parser that gives us a lot of flexibility. In a real game you may prefer to use the real INI format or XML or JSON or something custom.
- Besides updating the project files, the only thing you need to do with the UserSettings project for this assignment is to add two logging output messages
  - If a user runs the game successfully, s/he should be able to open the generated log file (in $(GameDir)) afterwards and see 1) what resolution width the game read from the settings.ini file and 2) what resolution height the game read from the settings.ini file.
  - You should be able to figure out where to add these two function calls and what they should look like
    - I have already added lots of logging error messages for when something in UserSettings goes wrong, and a regular informational message when the settings.ini file doesn't exist
    - You should add regular informational messages (not error messages), and you should do it when the resolution width is successfully set and when the resolution height is successfully set (and include the width or height value in the message)
  - Log files like this are really useful to try and figure out what went wrong with a game after-the-fact. There won't be very many requirements to add logging messages throughout the class, but I would strongly recommend you to get in the habit of starting to log things, both good and bad, so that you have a record of what happened during a program's run.

### Second Triangle

- In order to add a second triangle and draw a square there are two places you must change (and you must change this for each platform which means that there are actually four total places):
  - You must update the vertex data to include a second triangle
    - You must ensure that the vertex buffer has enough space for two triangles
    - You must fill in the data for the second triangle
      - Use the first triangle as an example. If you're having trouble figuring out which values to use get a piece of paper and draw the two triangles and their coordinates by hand. Remember that Direct3D is left-handed and OpenGL is right-handed! This means that you will add the same vertices to both platforms but they will be in a different order.
        - Note that the default in Direct3D is to only display triangles drawn with the correct order, and so if you do it wrong your second triangle won't show up. The default in OpenGL, however, is to display triangles regardless of order, and so even if you specify the wrong order you will still see the second triangle. Please try to get it correct for this assignment so that you don't have problems in future assignments when it does matter.
  - You must update the draw call to draw two triangles instead of one
    - The "draw call" is the function that happens every frame inside of Graphics:Render() that actually tells the GPU to "draw" something (i.e. fill in pixels based on some triangles)
    - In Assignment 01 the draw call says that only a single triangle will be drawn, and so even if you correctly add a second triangle to your vertex buffer (as discussed above) only the first one will show up on screen unless you also update the draw call to specify that two triangles should be drawn
- A correct assignment with no animation would look like this:
- assignment02_screenshot.png
- With animation it should still be a recognizable square. One example from my reference implementation is:
- assignment02_screenshotAnimated.png