# Assignment 3.01 - Introduction to Project Files & Animation

- Before trying to work on this assignment you should study and understand the following example programs:
  - [Solution Setup](./SolutionSetup.md)
  - [Static Library](./StaticLibrary.md)
  - [Platform-Specific Code](./PlatformSpecificCode.md)
  - [Include Direct3D](./IncludeDirect3D.md)
  - [Include OpenGL](./IncludeOpenGL.md)
  - [Asset Building](./AssetBuilding.md)

- The solution that you should start with to do this assignment can be downloaded here Download here. The files you will need to add to your new Graphics project can be downloaded here Download here.

## Requirements

- Rename the solution to match your GitLab project name
- Configure your game in cMyGame.h
- Create a Graphics static library and add it to the solution
  - Set up the dependencies, folder structure, and library inputs
- Set up the BuildAllAssets project to copy the appropriate shaders to the correct places
- Modify the vertex and fragment shaders so that the triangle position and color animates
  - Make sure that both platforms behave identically
- Your write-up should:
  - Show a screenshot of your game running
    - Try to pick one that shows the triangle's animating position and color (i.e. that is different from the default white triangle that the given code does)
  - Discuss briefly what you think the intended purpose of the class is, and tell us what specific things you personally are interested in learning or doing

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- There are no temporary files checked in
  - Nothing listed in the .gitignore file should be checked into git
  - This includes any vcxproj.user files that set up the working directory for debugging. Do not commit them!
- If you delete the temp/ folder, build the MyGame project only, and run the game:
  - A message should appear to the user saying that something has gone wrong
  - The game will exit but not crash
- If you delete the temp/ folder, build only the MyGame and BuildAllAssets projects, and run the game:
  - Everything should work correctly. This is a sign that your project dependencies are set up correctly, and is how Saurabh will correct your assignments.
  - Sometimes students get dependencies wrong and thier projects fail when built the first time but then succeed the second time. It is important to test building from scratch by deleting the temp/ folder to make sure that your dependencies are correct.
- You should be able to run the game by double-clicking the executable in Windows Explorer

## Details

### Solution Name

- Your solution's folder structure should look like this:
- assignment01_windowsExplorer.png
- The actual folder name (where I have "ReferenceImplementation" can be anything you want. Saurabh and I will never see it, because that is your git root directory.
- The actual solution file must match your GitLab project exactly, and so you won't be able to satisfy this requirement until your project is set up.
  - Don't worry, you can just rename this file with Windows Explorer and everything will still work.

### cMyGame.h Configuration

- You must change your game's window name
  - It can be anything you want, and doesn't have to include your name. I do ask that you leave in the platform and debug configuration like I have it, though.
- You should change your game's window class name, but this isn't necessary
- You may change the log file name or the icons if you are interested, but my advice would be not to spend time on this until you have finished the rest of the assignment

### Graphics Library

- You must create a new Graphics project for your solution
  - It must be named "Graphics"
  - It must be located in "Engine" in your directory structure, like this:
- assignment01_graphics_windowsExplorer.png
- It must be within the "Engine" folder in Visual Studio's Solution Explorer, like this:
- assignment01_graphics_solutionExplorer.png
- Add it by doing the following:
  - Right-click the Engine folder in Solution Explorer and choose Add->New Project...
  - Select Visual C++->Win32 Console Application
  - Click Browse... and change the location to Code/Engine/
  - Enter "Graphics" for the name
  - A popup window will appear. Click Next > (do not click Finish yet!)
  - Select "Static Library" and uncheck "Precompiled header"
- The very first thing you should do with your new project is to add the property sheets
  - If you are not sure how to do this review the Solution Setup Example
  - Remember that the order that you add the property sheets matters. Add SolutionMacros first, and then ProjectDefaults. Then add OpenGL to the Win32 configurations and Direct3D to the x64 configurations.
- Next, remove all of the files and folders that Visual Studio adds for you, and then delete them from disk
- Now, copy the provided Assignment 01 files Download Assignment 01 filesinto the Graphics library folder in Windows Explorer
- Then, add them to the project in Solution Explorer
  - Remember to set things up as the Platform-Specific Code Example explains. If you are not sure open that example solution again and make sure that you have the platform-specific files organized correctly, and that they are set to only build with the appropriate configuration.
  - You will lose points if you don't have this correct!
- Now you need to set up the project dependencies
  - You can figure out which other projects the Graphics project depends on by looking at the "Additional Dependencies" that you have set. If you haven't set any you should be able to figure out what you should set by looking at which header files are #included (for other projects in our solution) and by examining the Include Direct3D and Include OpenGL example solutions for graphic API libraries. If you don't know how to look at or set library dependencies then you should review the Static Library Example.
    - Remember that the Graphics project requires different libraries for different platforms. Make sure to look separately in x64 and x86 to make sure that you see every dependency!
  - Once you know which projects Graphics depends on, you can set up the official dependencies to make sure that the build order is correct. If you are unsure how to do this review the Solution Setup Example.

### Build Shaders

- You need to setup your asset build so that the shaders with the HLSL extension end up in the Direct3D $(BuiltAssetDir), and the shaders with the GLSL extension end up in the OpenGL $(BuiltAssetDir).
- If you are unsure how to do this review the Asset Building Example
  - In that example "asset1.txt" and "asset2.txt" get built. For Assignment 01 you need to do the exact same thing, so you can mostly copy and paste what the Asset Building Example does. The big difference you will have to be careful of is to set up the correct assets with the correct platforms.

### Triangle Position and Color Animation

- The code I have provided will render a white triangle:
- assignment01_screenshot.png
- You get to be creative and add some animation to the triangle! Here is one frame from my implementation that I captured as the vertices and color were animating:
- assignment01_screenshotAnimated.png
- I have provided instructions in the shaders themselves about how you can add animation, so you should look at the comments at the bottom of each shader.
  - Make sure that you are editing the source shaders in $(AuthoredAssetDir). You should modify the source, and then build the BuildAllAssets project to have them get to the correct place. If you accidentally author the built shaders you might get something that you really like, but since the file is in $(TempDir) it will be overwritten once stuff gets built again.
- You must make sure that the behavior in Direct3D and OpenGL is the same. That means that you must change both vertex shaders to use the same math, and both fragment shaders to use the same math. As the class progresses we will find ways of making things platform-independent so that you only will have to make the change in one place instead of two, but until we do that you must make sure to keep your game consistent between platforms.[]()