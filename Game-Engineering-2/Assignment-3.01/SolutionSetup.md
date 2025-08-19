# Solution Setup Example

## Temporary Directory

- If you build the solution and look at the generated files in Windows Explorer you will notice that all of them are under a temp/ directory. This is required for all code in this class! In order to get this to work, the following are needed:
  - **SolutionMacros.props** file. Visual Studio allows "property sheets" to be used to set project properties in a consistent way. This particular file is used to create user-defined macros that can be used in any project's properties.
    - In Visual Studio, select **View->Property Manager** to open the Property Manager window
    - You will see a list of projects in the current solution, and if you expand a project you will see subcategories of a project's build configurations
    - If you right click a project you can choose "Add New Project Property Sheet..." or "Add Existing Property Sheet...". You should copy the existing SolutionMacros.props file from this example solution and use it in your own solution, which means you would choose to add an existing property sheet.
    - If you expand one of the projects' configurations you will see "SolutionMacros" (as well as other properties that I have added above it and other properties that Visual Studio automatically adds below it). You can either double click it or right-click and choose "Properties" to edit the properties
    - Select "User Macros" in the tree view to the left
    - There you will see these macros:
      - **TempDir** - This defines the primary location for all temporary files for a given platform and build configuration
      - **IntermediateDir** - This defines where "intermediate" files will go. Intermediate files are those that Visual Studio creates as part of its build process. Note that it is defined in terms of $(TempDir).
      - **BinDir** - This is where Visual Studio will actually output "built" files (the "bin" is short for "binary"). In the case of the example solution this means the executable EXE files, but any output from a Visual Studio project (like static LIB or shared DLL libraries) will also go here.
      - **GameDir** - This is the actual folder that represents the finished game directory that you will give to a user. In this example solution there is nothing here except BuildMeLast.exe, but by the end of class your build system will put everything here needed to run the finished game (including your game's EXE as well as the data it will load).
      - **AuthoredAssetDir** - This is the folder where we will store "authored" assets. These are files that are checked into source control that are created by content authors working on your game (like MA files from Maya or PNG files from Photoshop). This folder doesn't exist in this example solution.
      - **BuiltAssetDir** - This is the folder where generated "built" assets will be placed. These are the data files that are actually loaded by the game when it runs. They can be the same as the authored assets, but in almost all cases in a real game they will not be; instead, the authored assets will go through some build process and be transformed into a format that the game can use more effectively and efficiently. Note that this macro is defined in terms of $(GameDir).
      - **ScriptDir** - This is where scripts used by the build process are located. This example solution has no scripts.
      - **SourceLicenseDir** - This is where licenses for external software are located. These files are checked into source control. This example solution has no licenses.
      - **TargetLicenseDir** - This is where the source licenses are copied to so that they are available to the end user. Note that this macro is defined in terms of $(GameDir).
- **<span style="color:#f00;">In this class you must not modify the existing required locations</span> (unless instructed to in a homework assigment)**. This does not mean that the existing required locations are ideal; instead, it makes it easier for me (and your fellow students) to know where files should be without having to learn a different convention for everyone.
- You may, however, add any other macros that would be useful to you
- **ProjectDefaults.props** file:
  - This file was modified the same way that you would set properties in a project, but by doing it in a project sheet it possible to set consistent properties in multiple projects.
  - The property sheet added to each project the same way that SolutionMacros.props was
    - Note that the order that property sheets show up in the Property Manager window are important! ProjectDefaults.props must be above SolutionMacros.props because it uses those solution macros. If you ever add property sheets in the wrong order you can right click one and choose either "Move Later in Evaluation" or "Move Earlier in Evaluation".
  - The advantage to this method is that if you decide to change your temporary directory's layout you can do so once to the property sheet and those changes will then be automatically applied to all projects. We will use ProjectDefaults.props for every project we create in this class.
- Copy the appropriate files to the final game directory
  - As the class progresses the way we will deal with this will become much more sophisticated and complex. This example solution provides a nice and simple introduction, however, by copying BuildMeLast.exe into $(GameDir).
  - Right-click the BuildMeLast project and choose "Properties"
  - In the tree view to the left select Configuration Properties->Custom Build Step->General
  - In the Command Line field I have added a command that calls the built-in "copy" program
    - Note that we are able to use the macros that we defined in SolutionMacros.props
  - In the Description field I have entered a message that will be shown in the output window when this build step is run. (Try building the solution and looking for the message! You should see it if your Visual Studio build output verbosity is set to NormalLinks to an external site. or more verbose, although you won't if it set to Minimal or Quiet.)
  - In the Outputs field I have entered the file that I expect this custom build step to generate. This is so that Visual Studio knows whether the custom build step needs to be run or not: If the file doesn't exist the step will always be run.
  - In the Additional Dependencies field I have added $(TargetPath). This is so that Visual Studio knows whether the custom build step needs to be run or not: If the file exists but it is older than the EXE that gets built into $(BinDir) then the step will be run to copy the updated EXE into $(GameDir).
- After following these steps you will be able to build your solution and have everything be placed in the appropriate location. You will also be able to always have a clean solution by deleting the temp/ directory.

### Debugging

- When debugging a program Visual Studio will default to using the project file's location as the working/current directory. In our class we will want to use $(GameDir) for the game (and $(BinDir) when debugging tools). The debugging information is stored in a user settings file, though, which we explicitly exclude from source control (see the .gitignore section below for more information). The steps to set the working directory correctly for the game project are as follows:
  - Right-click the project's name and choose "Properties"
  - In the tree view to the left select Configuration Properties->Debugging
  - Change Command to $(GameDir)$(TargetFileName)
    - (If you don't do this things will still work, but the debugger will be using the version in $(BinDir) instead of the version that users will be using in $(GameDir). This step can be considered optional.)
  - Change Working Directory to $(GameDir)
- When getting a clean solution from GitLab you will not be able to debug the game properly until the steps above have been done. You can assume that the TA and I will do them when we are looking at your assignments (do not change the .gitignore file to include the user settings).
 
### Project Build Order

- Over the course of this class we will end up with a solution that has many projects. Some of these "depend" on each other, which means that "Project B" can't be built unless "Project A" is built first. This example solution has three projects named in order to make it obvious what order they should be built in: "BuildMeFirst", "BuildMeSecond", and "BuildMeLast". The dependencies between projects are set up as follows:
  - Right-click a project (any project) and choose Project Dependencies...
  - A pop-up window will appear with two main tabs, "Dependencies" and "Build Order". Select the Dependencies tab.
  - There is a "Projects" drop-down that lists each of the projects in your solution. Below that is a "Depends on" window that shows all of the projects besides the selected on. You can select the check box for each project that the currently-selected one depends on.
    - BuildMeFirst doesn't depend on any other projects (both check boxes are unselected)
    - BuildMeSecond depends on BuildMeFirst. (This is a simple example to demonstrate how dependencies are set up and so this isn't really true; for a real example imagine a static library that must be built before an EXE that uses it can be.)
    - BuildMeLast depends on both other projects. (It would also be last if it only depended on BuildMeSecond, but if it only depended on BuildMeFirst it wouldn't necessarily be last because it and BuildMeSecond would be independent of each other.)
  - In Visual Studio you only specify the build dependencies, and the build order will then be chosen automatically as a result. If you wish to see the order that Visual Studio has decided to build your projects then select the Build Order tab.

### Platforms

- In our class we will build our assignments for two different platforms: 32-bit OpenGL and 64-bit Direct3D. The way we are doing this is artificial:
  - In a real game that supported both OpenGL and Direct3D the two could be switched between when running the same executable, rather than being separated into two different EXEs to run
  - The choice of a 32-bit vs. 64-bit executable has nothing to do with OpenGL or Direct3D (you can easily create a 64-bit OpenGL program and a 32-bit Direct3D program).
- The choice of doing it this way is for learning purposes, as well as a concession to the way that Visual Studio deals with platforms. In order for your solution to properly handle the different platforms it should match the following characteristics of the example solution:
  - Only two "solution platforms" should show up
    - The correct names for the two platforms are "x64" and "x86"
  - There is a property sheet for each platform, Direct3D.props and OpenGL.props
    - These add a preprocessor #define based on the platform: EAE6320_PLATFORM_D3D for Direct3D and EAE6320_PLATFORM_GL for OpenGL. You can see an example of these being used in each of the example projects.
    - The way you add these to new projects is almost the same as the way you add SolutionMacros.props and ProjectDefaults.props, but instead of adding the property sheet to the entire project you will only add it to the appropriate configurations
      - In the Property Manager, expand a project
      - Select both "Debug | x64" and "Release | x64" (by holding down control while clicking", and then right-click and choose "Add Existing Property Sheet..." to add Direct3D.props
      - Select both "Debug | Win32" and "Release | Win32", and then right-click and choose "Add Existing Property Sheet..." to add OpenGL.props

### .gitignore

- "git" is a source control program (like SVN) that we will be using for this class. You should only include the necessary authored files in your git repository, and exclude any generated temporary files. The mechanism git provides for this is by you include files named ".gitignore". The .gitignore file included in the example solution will probably be sufficient for the rest of the class. You can examine its contents in a text editor:
  - Temporary files generated by you
    - Everything in the $(TempDir) temp/ folder is ignored
  - Temporary files generated by Visual Studio
    - There is also a section that ignores temporary files that Visual Studio generates
  - Various other temporary files
    - I have added comments to explain each section
