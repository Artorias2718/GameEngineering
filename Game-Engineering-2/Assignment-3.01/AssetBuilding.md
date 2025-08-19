# Asset Building Example

## Program Flow

- The program does the following high-level operations in EntryPoint.cpp of the AssetBuildSystem project:
  - Initialization
    - Two environment variables are looked up and saved: The source directory for authored assets and the target directory for built assets. The way that these environment variables are set is explained in more detail below in Project Setup.
  - Iteration over every asset to be built
    - The list of assets to be built is passed as command line arguments
    - Doing it this way is problematic and hard to maintain. Soon we will develop a more sophisticated way of creating the list of assets to be built, but this is an easy and simple way to get started.
    - More details about where these command line arguments come from are given below in Project Setup.
    - Each asset is listed as a relative path, and this path is passed to a BuildAsset() function
  - The BuildAsset() function does the following steps:
    - Given the relative path of the asset, create the absolute path to the source asset and the absolute path to the target asset
      - "Source" and "Target" are pretty standard terms for build systems. The "source" in our case will be an authored asset (e.g. an image that can be manipulated in Photoshop, a mesh that can be manipulated in Maya, etc.). The "target" will be the corresponding asset after it has been "built", meaning that it is ready to be used in the game (e.g. instead of an image that can be opened by Photoshop the built texture would be in a platform-specific binary format optimized for a specific piece of graphics hardware, and instead of a mesh that can be opened by Maya the built mesh would have binary data that your specific game engine can use and any extraneous Maya information will be discarded).
      - In this example program there is a one-to-one correspondence between the source and target with the only thing differing being the location on disk (i.e. the target is created by copying the source file). In future assignments we will actually create tools that build assets so that your game uses a different file than was authored.
  - Given the source and target paths, decide whether the target needs to be built
    - If the source doesn't exist then the target can't be built
    - If the target doesn't exist then it must to be built
    - Even if the target already exists, though, it may be out-of-date. Every time that the source changes the target should be built again. On the other hand, the target should only be built if it has to be (for this simple example it doesn't matter, but in a real game building all assets can take hours).
      - To determine if the target is out-of-date the example program compares the time both the source and the target were last modified. If the source has been written to more recently than the target the target will be rebuilt.
    - Note that it in a real game's asset pipeline there can be much more complicated dependencies than just relying on file time.
  - If a target needs to be built:
    - Display a message to the user. (This will show up in Visual Studio's "Output" tab along with the other build messages.)
    - If necessary, create the directory where the target will go
    - Copy the source file to the target location
      - Again, please note that this copy step is the simplest form of "building" that there is, and is not typical of what will actually happen in a real asset build pipeline.
      - Soon we will learn how to build assets in a more traditional way where the targets will actually be different files than the sources

## Project Setup

- The way that we set up the asset build process will probably be a bit confusing, so 1) don't feel bad if it is and 2) please take the time to read through this next section and experiment with the example solution as often as you need to in order to get a better understanding of what's going on.
- Building the AssetBuildSystem project will create an executable file AssetBuildSystem.exe. This must happen first. Once AssetBuildSystem.exe exists then the BuildAllAssets project can use it when it gets built. In other words, building the entire solution will first build an executable and then use that executable to do something.
- The directory structure is set up the same way as described in the Solution Setup example program. There are two important macros in the SolutionMacros.props file that were not used in the Solution Setup example that will be used in this example, though:
  - AuthoredAssetDir - This defines where users should save their authored source assets
  - BuiltAssetDir - This defines where the build system will place the built target assets
- Note that $(AuthoredAssetDir) is outside of the $(TempDir), because the authored assets must be under source control. On the other hand, $(BuiltAssetDir) is inside of $(TempDir), because these are generated files and can always be re-created when the temporary directory is deleted (and thus should not be under source control).
- The BuildAllAssets project is empty (there's no code), but it uses a custom build step similar to the Solution Setup example program:
  - Right-click the BuildAllAssets project and choose "Properties"
  - In the tree view to the left select Configuration Properties->Custom Build Step->General
  - In the Command Line field you'll see the following:
    - "$(BinDir)AssetBuildSystem.exe" asset1.txt asset2.txt
  - This command line will run AssetBuildSystem.exe and pass the two asset names as its command line arguments.
  - In the Description field you'll see "Building Assets". This will be displayed in the Visual Studio "Output" tab when the custom build step is run (if your build output verbosity is set high enoughLinks to an external site.).
  - In the Outputs field you'll see "_ALWAYS_RUN_"
    - If you recall from the Solution Setup example program, the "Outputs" field is where you should list the files that are generated as part of the custom build step; this is how Visual Studio knows whether to run the step or not.
    - In our case we are abusing this system by listing a pseudo file that will never exist. This means that Visual Studio will always check to see if "_ALWAYS_RUN_" is updated, will never find it, and will always run the custom build step. In other words, this is a hack which will cause AssetBuildSystem.exe to run every time the BuildAllAssets project is built, and the BuildAllAssets project will always be built every time the solution is because of the same hack.
    - There are more elegant ways to solve this problem, but none that I know of that are this simple and suffice for the purposes of our class.
- (Important note: The following discussion about build output assumes that your Visual Studio settings are set to the "normal" level of verbosity. If you have a lower level set you won't see most of the following messages. If you want to temporarily increase the amount of build output you see just for experimenting with this example program, go to Tools->Options... in the menu, and select Projects and Solutions->Build and Run in the tree view at the left. There is a drop down for MSBuild project build output verbosity, and you can change that to Normal. This is not required for the class, and only affects your personal Visual Studio installation.)
- When you build the solution for the first time you should see output similar to the following:

```sh
- 6> CustomBuildStep:
- 6> Description: Building Assets
- 6> Building C:\Users\John-Paul\Documents\eae6320\2016 Fall\ExamplePrograms\AssetBuilding\Assets\asset1.txt
- 6> Building C:\Users\John-Paul\Documents\eae6320\2016 Fall\ExamplePrograms\AssetBuilding\Assets\asset2.txt
- If you then build the solution again you will only see:
- 1> CustomBuildStep:
- 1> Description: Building Assets
- As expected, built custom build step always runs, but we no longer see the output about asset1.txt or asset2.txt because they have already been built (remember that AssetBuildSystem.exe checks for this). If you look in $(BuiltAssetDir) you will see both of them there. Try deleting asset2.txt but leave asset1.txt there. If you build the solution you should see:
- 1> CustomBuildStep:
- 1> Description: Building Assets
- 1> Building C:\Users\John-Paul\Documents\eae6320\2016 Fall\ExamplePrograms\AssetBuilding\Assets\asset2.txt
```

- As expected, AssetBuildSystem.exe correctly noticed that asset2.txt was missing and had to be rebuilt, but asset1.txt was up-to-date.
- Now, try modifying one of the files in $(AuthoredAssetDir) (in other words, modify one of the source files in the Assets\ directory rather than one of the target generated files). If you build the solution again you should see that whichever one you modified got built and the other one didn't. This is because AssetBuildSystem.exe checked the last modified write time of the source file, saw that it was newer than the target file, and re-built the asset.

## Project Build Order

- In order for the BuildAllAssets project to run AssetBuildSystem.exe the AssetBuildSystem project must build first. Right-click a project and choose "Project Build Order...", and confirm that the build order is correct. Recall from the discussion in the Solution Setup example program that build order is determined by the projects' dependencies. In the window that opened to show the build order change the tab to "Dependencies" and note that BuildAllAssets depends on AssetBuildSystem. As our asset build system becomes more sophisticated over the course of the class it will be important to always remember to set the correct project dependencies or you may run into situations where one project will try to use the output of another project before it exists. (In cases like this the build will often fail the first time and then succeed the second time; this is bad and you will lose points for it.)

## Debugging

- The way that this solution is set up is very easy to use when it works correctly, but it can be confusing about how to debug problems when they arise. Remember that the BuildAllAssets project explicitly calls AssetBuildSystem.exe as a custom build step; if you want to debug AssetBuildSystem.exe, then, you will have to duplicate what BuildAllAssets does. There are two necessary steps to do this:
  - Set the appropriate command line
    - Right-click the AssetBuildSystem project and choose "Properties"
    - In the tree view to the left select Configuration Properties->Debugging
    - In the Command Arguments field add the list of desired assets that you want to debug. In order to debug the identical situation that BuildAssets is running you would enter:
      - asset1.txt asset2.txt
  - Set the required environment variables
    - Something that may not be immediately obvious is that AssetBuildSystem.exe explicitly checks environment variables to know where the source asset is coming from and where the target asset is going. These two environment variables are the macros we added to the project, $(AuthoredAssetDir) and $(BuildAssetDir). Note that these two macros exist only in terms of the Visual Studio solution and whatever projects we added the property file to, but they do not exist in your actual computer's list of environment variables.
    - When the BuildAllAssets project's custom build step is run those two environment variables exist, and so AssetBuildSystem.exe works the way that it should
      - (Note that the reason they exist is because I checked "Set this macro as an environment variable in the build environment" when I added the macros to the SolutionMacros property sheet)
  - When you manually debug AssetBuildSystem.exe, however, those two environment variables do not exist. In order to make them exist, do the following:
    - Right-click the AssetBuildSystem project and choose "Properties"
    - In the tree view to the left select Configuration Properties->Debugging
    - In the Environment field add the list of desired environment variables that you want to debug
    - You will need to add two, and the easiest way to do this is to click in the field, click the drop down arrow that appears at the right of the field, and choose <Edit...>
    - A new popup window will appear that lets you add things on multiple lines. Environment variables can be set using the following syntax:
      - key=value
    - The specific variables you will need in order to debug AssetBuilder.exe will look like this:
      - AuthoredAssetDir=$(AuthoredAssetDir)
      - BuiltAssetDir=$(BuiltAssetDir)