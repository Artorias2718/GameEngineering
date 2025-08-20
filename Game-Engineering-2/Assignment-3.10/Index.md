---
---

# Assignment 3.10: Effect Files

- Create a human-readable Effect file format
  - For this assignment an Effect needs:
    - The (source) path of a vertex shader
    - The (source) path of a fragment shader
  - In a future assignment we will also add render states to this file format
- Update AssetBuildSystem.lua to handle Effects
  - You should add two sections of code in the provided fileDownload provided file
    - The first section (that sets environment variables) should replace the existing section in your file
    - The next section (that adds an external interface function) is new and should be added at the end of your file
- Create a new Asset Info Type for effects
  - As always you must add a GetBuilderRelativePath() function, and if you desire you may add a ConvertSourceRelativePathToBuiltRelativePath() function
  - New for this assignment, however, you must also add a RegisterReferencedAssets() function
- Create an EffectBuilder project that reads in a human-readable Effect file and writes out a binary Effect file
  - You should first update the AssetBuildLibrary and AssetBuildSystem projects with the provided filesDownload provided files
    - (AssetBuildLibrary will have new project dependencies after you update that you will have to take care of. Can you figure out what they are? Looking at which files are #included should help.)
- Create an Effect file and add it to the Asset Build System
  - Add the new Effect file to AssetsToBuild.lua
  - Remove the shaders from AssetsToBuild.lua
    - If you have correctly implemented the RegisterReferencedAssets() function for Effects then the shaders will automatically get built
  - (Note that once you start working with actual assets you should be careful when making changes to AssetBuildSystem.lua. Because we have implemented our own Asset Build System outside of Visual Studio's knowledge, when you make changes to AssetBuildSystem.lua it is good practice to delete your generated assets.)
- Update your run-time code to load the binary Effect file
  - Your code prior to this assignment should have had a load function for Effects that took the vertex and fragment shader paths as input. In this assignment you should change the load function so that it instead takes the path of your binary Effect file, and then you will update the implementation to:
    - Load the binary Effect file
    - Extract the vertex and fragment shader paths from the loaded binary data
    - Use those shader paths exactly the same way you previously used the paths that were input parameters to the function
- Make the Effect part of the submitted draw call
  - Instead of having a single Effect, allow each draw call to use a different one
  - You will have to change your draw call submission function to accept an Effect
  - You will have to update your list of submitted draw call data to include an Effect
  - Instead of loading a single Effect internally in Graphics.[platform].cpp (get rid of that one) your game code will have to load Effects and associate them with objects (just like you do with meshes)
  - When you iterate through each draw call you will have to set/bind the effect before setting the draw call constant buffer data and drawing the mesh
- Create a second Effect and assign it to one of your objects
  - You must create at least one new shader (either vertex or fragment) to use in this Effect, and it must animate over time (see the Details section for ideas on how to do this)
  - If at all possible try to come up with something that you can use for your final project
- Make your Graphics::RenderFrame() function platform-independent
  - This function must be moved to a Graphics.cpp file (there can only be a single definition for it)
- Your write-up should:
  - Show us a human-readable Effect file
  - Show us the binary version of the same Effect file (a screenshot from a hex editor is fine AS LONG AS IT'S READABLE)
    - Explain how your run-time code knows where the second path in the file starts
    - Tell us whether the paths you store are relative to $(GameDir) or $(BuiltAssetDir). Explain why you made this decision. (You must explain the advantages and disadvantages of your choice or else you will lose points.)
  - Show us how you extract the two paths at run-time
  - Show us at least two screenshots that show your object with the custom Effect in two different states as it animates
    - Explain how your custom Effect will be used by your final game
  - Show us your platform-independent Graphics::RenderFrame() function. Isn't it beautiful?

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- There should be no shaders listed in the AssetsToBuild.lua file
  - If you delete $(TempDir) and build assets then shaders should be built (because they are referenced by effects)

## Details

### RegisterReferencedAssets()
- This function must examine the effect file and register the two shaders that the effect references
- The function will get a relative path to the source effect as input. That means your implementation should look something like the following:

  ```lua
  NewAssetTypeInfo( "effects",
    {
      RegisterReferencedAssets = function( i_sourceRelativePath )
        local sourceAbsolutePath = s_AuthoredAssetDir .. i_sourceRelativePath
        if DoesFileExist( sourceAbsolutePath ) then
          local effect = dofile( sourceAbsolutePath )
          -- EAE6320_TODO Get the shader paths from the effect table and then do something like:
          RegisterAssetToBeBuilt( path_vertexShader, "shaders", { "vertex" } )
          RegisterAssetToBeBuilt( path_fragmentShader, "shaders", { "fragment" } )
        end
      end
    }
  )
  ```

## Binary Effect Files
- In this assignment the only two pieces of data that an Effect needs are the vertex shader path and the fragment shader path
- The shader paths that are in your human-readable source Effect file should be source shader paths, but the shader paths that are in your binary built Effect file should be the paths to the binary built shaders. That means that you will have to convert them in your EffectBuilder tool.
  - The provided files give you a new ConvertSourceRelativePathToBuiltRelativePath() utility function in AssetBuildLibrary that can be used to do this
- Even though the paths themselves are text, when they are part of a binary file the priority you need to keep in mind is what the best format is for reading them in at run-time. Some things to consider:
  - You only need the paths while loading the file
    - (In other words, you don't need to store them. When you extract them from the loaded binary data the pointers can point into that binary data as long as you only use those pointers during load-time.)
- NULL termination
  - You don't technically need to terminate the strings with a NULL character. However, in order for char arrays to be used as strings at run-time a terminating NULL is necessary. If your file doesn't include a terminating NULL then you will have to add one at run-time, and that is expensive. Since a terminating NULL is only one byte it is definitely worth adding to the file so that the run-time can use the data directly as a string.
- How does the run-time know where the fragment shader path starts?
  - One option is to calculate this at run-time. (Can you think of how?)
  - Another option is to add this information to the binary file
    - If you do this then the size of the file increases and you have to extract the information at run-time, but you save the run-time cost of having to calculate where the fragment shader path starts
    - How small can you make the information that tells where the fragment shader path starts?
- What should the path look like?
  - In the Asset Build System the paths are relative to $(BuiltAssetDir), but at run-time when you load a file the current working directory is $(GameDir). That means that you have two options:
    - Store the paths relative to $(BuiltAssetDir), and at run-time add a prefix to them after extracting them
    - Add the prefix at build-time and store the paths relative to $(GameDir)
  - One option decreases file size but increases run-time cost. The other option increases file size but decreases run-time cost. Which do you think is better?

## Creating a New Effect

- You may create either a new vertex shader, a new fragment shader, or both
- Start by copying an existing one. Most of the shader code should remain identical!
- The easiest things to change are:
  - The local position
  - The color
- The easiest way to make things animate is:
  - Using sin() or cos() with g_elapsedSecondCount_total
  - (This should be very similar to what you did in Assignment 01)
- Remember that a sinusoid varies between -1 and 1. If you are modifying position or color (as I am suggesting) then these values are probably much too big. Multiply the result by some smaller number to end up with just a small modification.
- Remember to make the shader change for both platforms! Your custom effect should look identical on both platforms.

## Platform-Independent Graphics::RenderFrame() Function

- If you have completed all of the previous assignments successfully your two RenderFrame() functions will already be identical except for two things:
  - The code at the first that clears that backbuffer
  - The code at the end that swaps the backbuffer with the front buffer
- You will need to make platform-independent interfaces for those two things (and keep the platform-specific implementations, of course), and then your two functions will be identical
- Even when the two RenderFrame() functions are identical, however, you may find that it's not trivial to move them to a single Graphics.cpp file
  - If you have followed my style then you will have several static variables (local to a file) that your RenderFrame() function relies on, and if you only move that function out of Graphics.[platform].cpp and into Graphics.cpp then you will get errors.
  - If this happens, DON'T PANIC!
  - The static variables should mostly be platform-independent by now. Start moving them over to the platform-independent Graphics.cpp file.
  - Once the static variables are gone you may also get errors from the Initialize() and CleanUp() functions.
  - If this happens, DON'T PANIC!
  - Most of the Initialize() and CleanUp() functions should also be platform-independent by now, because they are initializing and cleaning up objects that you have encapsulated with platform-independent interfaces. You should be able to move Initialize() and CleanUp() over to Graphics.cpp as well, with only a few platform-specific exceptions.
  - What you should find is that the things that are still platform-specific are related to a problem you have already solved: How can your representation of a mesh get access to the Direct3D device to make a draw call? What you need to do is to use those same principles to solve your current problems:
    - Most of the Graphics::Initialize() code is platform-independent, but there is some that is specific to Direct3D and some that is specific to OpenGL. Can you make a platform-independent interface for initializing platform-specific stuff? If so, call that function from your platform-independent Initialize() function and create a platform-specific implementation.
    - Do the same for the Graphics::CleanUp() code