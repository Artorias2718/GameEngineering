# Assignment 3.07: Maya Exporter Plugin

## Requirements

- Install Maya 2017 (64-bit)
  - Install the Maya SDK
- Set the following two environment variables on your machine (you will have to close and restart Visual Studio after doing this):
  - MAYA_LOCATION
    - This is where you installed Maya on your machine
    - The default location for this is probably:
      - C:\Program Files\Autodesk\Maya2017
    - <span style="color:#f00;">Note that there is no trailing slash!</span>
- MAYA_PLUG_IN_PATH
  - This tells Maya where to look for plug-ins, and is arbitrary. You can choose whatever location you like on your machine, and this is where your solution will copy your plugin to so that Maya can find it.
  - The most standard location for this (using your own user account, of course) is probably:
    - C:\Users\John-Paul\Documents\maya\2017\plug-ins
  - <span style="color:#f00;">Note that there is no trailing slash!</span>
- Add the provided MayaMeshExporter project Download provided MayaMeshExporter projectto your solution
- Change the file name of the plug-in that gets built
- Ensure that a 64-bit plug-in is built, regardless of what platform is selected
- Complete any missing code in MayaMeshExporter (by replacing EAE6320_TODO)
- Recreate your floor mesh in Maya and use the exported version in your game
  - Make sure to save (and add to source control) the native Maya ASCII file (with extension .MA) in addition to the exported mesh so that if you need to change your format and re-export it in future assignments you will be able to!
  - (You should do this with every mesh in this and future assignments. Save the native MA file in case you need to make changes or re-export in the future!)
- Create and export a sphere mesh in Maya and use it in your game instead of your hand-created cube mesh
  - This particular mesh file must be named "sphere" (with whatever extension you choose) so that it is easy for Saurabh to export something different and see whether it shows up in-game.
  - The user must be able to move it with the keyboard
  - It should have have vertex colors assigned by you in Maya that are different than the floor's color
    - (You can have it be a solid color if you'd like, but you can also have fun and make different parts have different colors. The only requirement is that its color is different than the floor so that it is easy to tell the two objects apart.)
  - It doesn't have to rotate like the cube did in Assignment 06, although it can if you like
- Create and export at least one other mesh in Maya that has a different shape, and create at least one other object in your game that uses this mesh
  - It can be any color that you like
  - It can rotate or move if you like, but doesn't have to
  - You must have at least one object in addition to the sphere and floor, but you can have make more if you want to be creative
- Your write-up should:
  - Tell us what the MayaMeshExporter project's dependencies are (what projects does it depend on and which projects depend on it) and why
  - Show us a screenshot of the Maya Plug-in Manager window with your plugin loaded
  - Show us a screenshot of you debugging your plugin. (In other words, I want a screenshot taken of a Visual Studio window while it's attached to Maya, showing the little yellow arrow and some variable values.)
  - Show us a screenshot of your beautiful scene!

### Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- If you have already built and run your game successfully and then create an arbitrary shape in Maya, export it to overwrite your "sphere" mesh, do an incremental build of the solution, and run the game, you should see that shape in-game
  - This should work automatically. You should not have to delete your temp folder first! If it doesn't work automatically something is wrong with your asset build and you must fix it!

## Details

### Installing Maya

- My understanding is that you should have access to Maya through the University. If not, you should be able to get it as a student from [here  &#128193;](http://www.autodesk.com/education/free-software/maya)
- The Maya installation is missing some of the SDK files
  - You can read the instructions for installing them at $(MAYA_LOCATION)\devkit\README_DEVKIT_MOVED.txt
  - The [link &#128193;](https://apps.autodesk.com/MAYA/en/Home/Index) in that text file doesn't take me directly to the download, and so you may have to search for "devkit" on that page to find it. Once you have downloaded it follow the instructions in the first paragraph hereLinks to an external site. and make sure that you have the correct folder structure.

### Completing the Plug-In

- Change the name of the plug-in
  - Do this first!
  - Right-click the project and choose Properties
  - In the tree view to the left choose Configuration Properties->General
  - Change Target Name to use the following:
  - eae6320_gitlabProjectName
  - Where "gitlabProjectName" is the name that I have assigned your project on gitlab. (For me this would be "eae6320_ownby_johnpaul".)
  - Add a "_DEBUG" suffix at the end for debug configurations (for me this would be "eae6320_ownby_johnpaul_DEBUG"
- Make sure that a 64-bit plug-in is always built
  - Select the arrow next to the current platform and choose Configuration Manager...
  - Find the MayaMeshExporter project
  - Change Active solution platform to x86
  - Make sure that the MayaMeshExporter project's platform is x64 (rather than Win32) when the Active solution configuration is both Debug and Release
- Fill in the MayaMeshExporter code everywhere there is a EAE6320_TODO (do a global search, because some of the places may compile correctly)
  - You will have to change the description of the plug-in that Maya displays so that it shows your name
    - (After this class is over you may change this to something better, but please put your name first like the assignment asks for to make things easier on Saurabh)
- You will have to change the default file extension to whatever you have decided for your human-readable mesh file
  - This allows the user (you) to just enter in the name of the mesh and let Maya choose the correct extension by default
- You will have to complete the ```WriteMeshToFile()``` function to generate your human-readable format
  - There are two pieces of data in the function that you care about:
    - A std::vector of sVertexInfo structs called i_vertexBuffer
      - You can see this struct defined at the top of cMayaMeshExporter.cpp
      - It has an sVertex_maya struct, which has the information relevant to the mesh file (i.e. the same information that could be contained in the run-time's sVertex struct), and then some other information needed for processing in the plug-in but that shouldn't be exported
        - (Or, put another way, the only information that should go in your exported file is what is in sVertex_maya.)
      - The order of these vertices is what the indices in i_indexBuffer refers to, so you don't need to do anything besides write these out in the same order that they are stored in the std::vector
    - A std::vector of size_ts called i_indexBuffer
      - This contains an array of indices into i_vertexBuffer that represent the mesh's triangles
      - There is nothing tricky here; these are the same indices that you are familiar with from previous assignments
    - (In addition there is a std::vector of sMaterialInfo structs. You should ignore that for this assignment.)
- I have provided you an example of opening the human-readable file and writing out the main Lua table that gets returned, and so you just need to fill in the actual data in your specific format. I am using std::ofstream which I find to be easy to use for generating text files, but you may change this to use fprintf() or any other method if you prefer.
- You will notice that the sVertex_maya struct has quite a bit more information for each vertex than we have learned about in our class so far. You have two options:
  - Ignore the extra information and just output position and color
    - The advantage of this would be keeping things simple and understandable. The disadvantage would be that if we want to add more information in future assignments you will have to re-export all of your meshes from Maya.
  - Output the extra information even though it will be ignored for now
    - The disadvantage of this is that it makes your human-readable file more complicated (although you will need some of this information eventually). The advantage is that it's all there and you potentially won't have to re-export your meshes if a future assignment wants to use any of it.
- Notice that you are outputting raw text. This means that you can format the Lua any way that you'd like, and you can also add comments if you want. (For example, you might decide that it would be useful to add a comment about how many vertices there are total; you wouldn't make this part of the actual format because it is implicit in the data itself, but as a comment it might help you as a human trying to debug a problem.)

### Loading the Plug-In in Maya

- In the menu, choose **Windows->Settings/Preferences->Plug-in Manager**
- If you have correctly set up your MAYA_PLUG_IN_PATH environment variable then you should see that location at the top, and if you have correctly built your plug-in you should see it there
- Click the "Loaded" box to load it. (Uncheck the box to unload it.)
- If your plug-in was successfully loaded you can click the "i" to see information about your it
- While developing it is really useful to keep the Maya scene open, and keep loading and unloading your plug-in to test it (unload, build, load again). Once this assignment is complete and you know that your plugin works, however, you will probably find it more convenient to click the "Auto load" box. This will make Maya always load your plug-in whenever it loads.

### Debugging the Plug-In

- Once Maya is running you can "attach" a Visual Studio debugger to it, after which you can debug Maya just like your own program!
  - (When you debug your game Visual Studio is actually doing this step for you automatically, but you could also run your game manually and then attach Visual Studio to it)
- In Visual Studio's menu choose DEBUG->Attach to Process...
- You should be able to find maya.exe in the list of available processes. (I type "m" to find it faster.)
- Once it's selected choose "Attach"
  - Make sure that Visual Studio shows "Native code" that in the "Attach to:" window. If it shows "Python code" instead you will have to change it manually to native code.
- Try setting a breakpoint in your plugin
  - If Maya has loaded it you should see the standard red breakpoint circle
  - If you only see an outline of a circle with an exclamation mark you can hover your mouse over it and it will tell you that no symbols have been loaded. If you go to the Plug-in Manager in Maya and load your plugin the breakpoint should change to the standard red circle.
- Once the debugger is attached and your plugin is loaded then breakpoints should be hit just like you are used to with your own programs. When you export a mesh Visual Studio should stop at your breakpoints and then you can step through and debug. Cool!
  - (For developing and debugging remember to use a debug version of your plugin.)

### Creating and Exporting Meshes

- To create the floor plane:
  - Choose **Create->Polygon Primitives->Plane** from the menu
    - (If you want to be able to specify interactive options for creating objects select Create->Polygon Primitives->Interactive Creation in the menu)
  - Choose **Windows->General Editors->Attribute Editor**
  - The Attribute Editor will show information about the currently selection. If you have the plane selected you should see a number of tabs, including "polyPlane1" and "pPlane1" (these names might be different if you have already created other things in your scene)
  - The polyPlane tab allows you to edit attributes specific to the plane shape. You should change the width and height to match your existing floor plane, and change subdivisions width and height to 1.
  - The plane tab allows you to edit attributes about where this instance of the mesh is in the world (the "transform"). You should set the "Translate" to match your existing floor plane (which will be (0,0,0) if you followed the instructions in Assignment 10).
  - To edit vertex colors do the following:
    - When the plane is selected hold down the right button when your mouse is above the object (note that you hold down the right button rather than right-clicking it), and choose "Vertex"
    - This puts you into a vertex-editing mode. (When you are done with this step and hold down the right button and choose "Object Mode" to go back to the default mode.)
    - You should see the four vertices of the plane, and you can select them. It's possible to click to select a vertex but I often find it easier to click and drag to select an area of vertices.
    - Once you have the vertices selected that you want to change go to MeshDisplay->Apply Color in the menu but select the square to the right of it. (When you see a square like this in Maya it means you can open up a window with extra properties.)
    - You will see the Apply Color Options window. Click the color rectangle and choose a color that you like.
    - When you are ready click "Apply". This will assign that color to the vertices that you have selected.
    - (If you only see the outline of the plane you are probably in "wireframe" mode. Look for the "Shading" menu at the top of the 3D viewport (not the main menu, but the window that shows the 3D scene), and change it to "Smooth Shade All".)
  - Export this floor mesh and compare the generated file with the one that you authored by hand. If they are the same then you have created a successful exporter! If there are unintentional differences then you will need to debug and figure out why.
- To create a sphere:
  - Choose **Create->Polygon Primitives->Sphere**
- Make sure that all of the meshes you create and export are centered around the origin and a reasonable size relative to the other objects.
- Here is an example of a sphere with three different vertex colors:
- assignment07_sphere.png