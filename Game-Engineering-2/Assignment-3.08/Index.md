# Assignment 3.08 - Platform-Independent Shaders, AssetList File

## Requirements

- Add the external mcppLinks to an external site. project (from the included files Download included files) to your solution
  - It doesn't depend on any other projects
  - It is only needed for the OpenGL platform, and can be disabled for Direct3D configurations
- Create a single vertex shader file and a single fragment shader file
  - Choose a new file extension that makes sense for a platform-independent shader
- Update the AssetBuildLibrary and AssetBuildSystem projects with the included filesDownload included files
  - The AssetBuildLibrary has a new base class. You can add the H and CPP files, but I don't believe any other changes are necessary.
  - The AssetBuildSystem uses the same files as before, but all of them have changed. I would advise just copying the new ones over your old ones, but if you want you may preserve your implementation of the C files that can be called by Lua.
- Copy the updated AssetBuildSystem.lua script to your solution
- Add info for meshes to AssetBuildSystem.lua
  - Search for EAE6320_TODO. Add info for building meshes, using the info for shaders as a guide
  - At a minimum you will need to provide a GetBuilderRelativePath() function
  - If you want to use a different subdirectory, name, or extension for meshes (or shaders), however, you will also have to provide a ConvertSourceRelativePathToBuiltRelativePath() function
    - This isn't hard to do, but you will have to read the code and comments to figure out how
- Copy the new AssetsToBuild.lua script to your solution, and add it to your solution so that it can easily be found from Visual Studio
- Update the BuildAllAssets project's Custom Build Step
  - It now calls AssetBuildSystem.exe with a single argument, which is the absolute path to AssetsToBuild.lua
    - (Make sure to specify the absolute path in a way that will work regardless of where the solution is located on disk)
  - Update AssetsToBuild.lua for your own game
  - I have given you an example file that has a sphere and a floor mesh and a vertex and fragment shader
    - Change the file names to what you use for your authored assets
  - Don't forget to change the subdirectories to match what you have
- Add whatever third mesh you use
  - Add the provided ShaderBuilder project to your solution
  - When setting up the project dependencies make sure to look at the OpenGL linker inputs. OpenGL depends on more projects than Direct3D, and so you will need to specifically look at those to make sure that you don't miss any.
    - The only code that you should need to change to get this project to work is the enumeration for shader type
      - The provided code assumes that such an enumeration exists and that it has values for Unknown, Vertex, and Fragment. You will either need to update this code to use your own enumeration if you have one, or change the shader builder code to use a different scheme to indicate which kind of shader should be built based on the input arguments.
- ```#define EAE6320_GRAPHICS_AREDEBUGSHADERSENABLED``` in the appropriate place and with the appropriate default behavior
  - Where do graphics #defines like this go? (There is already one that determines whether the graphics device should have debug info)
  - By default this should be #defined in debug builds but not release builds, but this should be easy to change during development if you ever need to debug a shader in a release build. The existing #define is a good example of how to do this.
- Add the provided MeshBuilder project to your solution
  - For this assignment the MeshBuilder will still only copy the human-readable authored mesh files to $(GameDir)
  - You will have to replace the EAE6320_TODO in cMeshBuilder.cpp with code that copies the source file to the target file, and properly use the result to decide whether to output an error and what value to return.
- Update your code to load the shaders properly
  - Your OpenGL code shouldn't have to change except to look for the new platform-independent file extension
  - Your Direct3D code can now load the compiled shader directly rather than compiling at run-time
- Your write-up should:
  - Tell us whether you use the same or different file extensions for authored shaders and built shaders, and explain why this choice makes the most sense to you
  - Tell us where you added AssetsToBuild.lua in Solution Explorer, and explain why you chose that location
  - Tell us which projects depend on ShaderBuilder and MeshBuilder, and explain why. (Remember what a build dependency actually means!)
  - Explain what advantages AssetsToBuild.lua offers (compared to how we have built assets in previous assignments)
  - What is the difference in file size between the built vertex shader in Debug and Release in Direct3D? Explain why the sizes are different. (Don't just mention what causes the difference, but also explain why this difference is desirable; what are the priorities for a debug build compared to the priorities for a release build?)
  - What is the difference in file size between the built vertex shader in Debug and Release in OpenGL? Why are the sizes different even though these files are still source code? (You will need to open the files in an editor to visually look at the differences.)

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- If you add a bug to a shader (like adding "sdf" on an empty line) and then build assets 1) the build should fail, 2) an error message should be displayed in Visual Studio's Error List window, and 3) double-clicking that error should open the file and take you to a line close to where the error is (in Direct3D this will be in the authored source file, but in OpenGL it will be in the built file)

## Details
### Creating a Single Shader File

- Use preprocessor ```#ifdef```s to do this by platform (using the same ```EAE6320_PLATFORM_D3D``` and ```EAE6320_PLATFORM_GL #defines```  that we have used in C++ code)
- In future assignments we will make the code more platform-independent, but for this assignment you can keep the code for the two platforms completely separate. Create a section for one platform and copy the entire contents of the old shader file into that section, and then create another section for the other platform and do the same.
- The ShaderBuilder will #define the appropriate platform so that the preprocessor statements work correctly

### Loading Binary Shaders

- The following code shows how a compiled fragment shader could be loaded in Direct3D:

  ```cpp
  bool LoadFragmentShader()
  {
    bool wereThereErrors = false;
  
    // Load the compiled shader
    eae6320::Platform::sDataFromFile compiledShader;
    const char* const path = "data/Shaders/fragment.shader";
    {
      std::string errorMessage;
      if ( !eae6320::Platform::LoadBinaryFile( path, compiledShader, &errorMessage ) )
      {
        wereThereErrors = true;
        EAE6320_ASSERTF( false, errorMessage.c_str() );
        eae6320::Logging::OutputError( "Failed to load the shader file \"%s\": %s", path, errorMessage.c_str() );
        goto OnExit;
      }
    }
    // Create the shader object
    {
      ID3D11ClassLinkage* const noInterfaces = NULL;
      const HRESULT result = [D3DDEVICE]->CreatePixelShader(
        compiledShader.data, compiledShader.size, noInterfaces, &s_fragmentShader );
      if ( FAILED( result ) )
      {
        wereThereErrors = true;
        EAE6320_ASSERT( false );
        eae6320::Logging::OutputError( "Direct3D failed to create the shader %s with HRESULT %#010x", path, result );
        goto OnExit;
      }
    }
  
  
  OnExit:
  
    compiledShader.Free();
  
    return !wereThereErrors;
  }
  ```

- Loading the vertex shader is the same as shown above except that the compiled shader needs to be available in order to create the vertex input layout. Instead of using an ID3D10Blob* you should be able to use an eae6320::Platform:sDataFromFile.