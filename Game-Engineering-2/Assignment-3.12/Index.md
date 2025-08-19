# Assignment 3.12 - Texture Files

## Requirements

- Add texture coordinates
  - Export UVs from Maya
  - Add a texture coordinate to your C++ vertex
  - Write texture coordinates from your MeshBuilder
  - Add a texture coordinate to your shaders
- Update the OpenGlExtensions project with the provided codeDownload provided code
  - If you haven't changed this project then you can just copy the provided files over the old ones
- Add the provided Download providedexternal DirectXTex project
  - This is an Open Source project made available by MicrosoftLinks to an external site.. The DirectX SDK used to provide a "D3DX" extension library, but Microsoft has been moving towards releasing the utility functions as external open source projects.
  - This project doesn't depend on anything else in your solution.
- Add the provided Download providedTextureBuilder project
  - You shouldn't have to change any of the files in this project to use it, but you will have to set up its build dependencies. You should know how to figure out which projects it depends on.
  - Add the provided Download providedcTexture class to your Graphics project
- Add an asset type info to your AssetBuildSystem.lua
  - You must implement a ConvertSourceRelativePathToBuiltRelativePath() function:
    - There are different extensions that the authored assets can have (e.g. BMP, JPG, PNG, TGA), but all of the built assets must have the extension DDS
- Add the provided default.tga Download provided default.tgaimage to your AssetToBuild.lua
  - (It would be a good idea at this point to test that everything is working. You should be able to open the built default.dds in the DirectX Texture Tool, which can be found at $(DXSDK)Utilities/bin/x64/DxTex.exe.)
- Create and set/bind a sampler state for each platform
- Add a color texture to materials
  - Add a way to specify a path to the color image in your human-readable material file
    - This must be optional. (If no path is provided default.tga will be used implicitly.)
- Add code in your material asset info type (in AssetBuildSystem.lua) to register a texture to be built if it is in a material file
  - (This means that default.tga is the only texture you have to explicitly add to AssetsToBuild.lua. All other textures should be built automatically because they are assigned to a material.)
- Update your MaterialBuilder to get authored color image path and add the built texture path to the binary material file
  - It is ok to specifically look for the specific color image, and you may assume that the binary material file will always have one and only one color texture path.
  - If no authored color image path is provided the built default texture path should be added instead
    - (In other words, the binary file should always have a texture path, even if the authored file doesn't)
- Update your run-time material representation to have a texture
  - Your material should have a cTexture
  - While a material is being loaded it should extract the path to the texture from the binary file and use that to load the cTexture
  - When your material is set/bound it should also set/bind the cTexture
    - cTexture::Bind() requires an ID as an input parameter. This is the register or ID defined in the shader. In our class you may hard-code that number to be zero, since we will always only have a single texture in any shader.
- Add texture sampling to at least one of your fragment shaders
- Your game must have at least two materials with different textures
  - NOTE! My expectation for your final game is that everything will have a texture. The requirement for two is just for this assignment to get you started.
- Your write-up should:
  - Show a screenshot of your game with your beautiful textures!
    - (Feel free to show more than one; your game should be starting to get more interesting to look at and readers always enjoy pictures)
- Show an example of a human-readable material file with a color image specified
  - Explain how you would add a second image path if we created an effect with two textures
- Show the binary version of the same material file (a screenshot from a hex editor is fine if it's readable)
  - Tell us what changes you had to make to deal with having two paths (the effect path and the texture path)
  - Tell us what changes you would have to make if you had to support more than one texture
- Show us the run-time C++ code that extracts the material data from the binary file
  - Explain how you figure out where the effect path starts and where the texture path starts
- Describe the current state of your final project game
  - Are there any new features since last week?
  - Have your plans changed at all? Have any features been changed or cut? Have you decided to add anything?
  - What do you have left to do?

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- If you open a material and delete (or comment out) the assigned texture path:
  - If you build the solution the changed material should build without errors
  - If you run the game it should run without errors, and the material should now use a default white texture
- All of your built textures should have the DDS extension (e.g. the provided default.tga image should be named default.dds in $(BuiltAssetDir))

## Details

### Texture Coordinates

- Exporting from Maya
  - Your exporter should export the "u" and "v" values from each sVertex_maya
    - This is a two dimensional texture coordinate. You could use a dictionary and label them "u" and "v", but you may also use an array without labeling each. Just like an array of three numbers as a position is understood to be XYZ and an array of three or four numbers as a color is understood to be RGB or RGBA an array of two numbers as a texture coordinate will be understood by everyone with domain knowledge to be UV.
- C++ Code
  - Update the sVertex struct
    - You should add two floats
      - (16-bit floating point values ("halfs") almost always give enough precision for texture coordinates. It is fun to support this, but since the compiler doesn't natively support these we will keep things simple and use standard 32 bit floats)
    - You will have to decide where the best place to add two floats in the struct is. Will there be any alignment issues?
- Update the Direct3D Input Layout code
  - Use "TEXCOORD" for the semantic name
  - Use 0 for the semantic index
  - You should know what the other values should be
- Update the OpenGL VertexAttrib code
  - Use 2 for the location
  - You should know what the other values should be
- MeshBuilder
  - Extract the texture coordinate information from the human-readable file and assign it to the updated sVertex
  - Texture coordinates are different in Direct3D and OpenGL!
    - In OpenGL the origin is at the bottom left of a texture, and larger values of V specify coordinates closer to the top
    - In Direct3D the origin is at the top left of a texture, and larger values of V specify coordinates closer to the bottom
  - Maya uses the OpenGL convention. If your human-readable file exports the UVs as Maya reports them then you should keep them that way for your binary OpenGL mesh but reverse them for your binary Direct3D mesh.
  - To convert a V coordinate from the OpenGL convention to the Direct3D convention:
    - float d3d_vCoordinate = 1.0f - gl_vCoordinate;
  - (Having different UVs has interesting repercussions when trying to write platform-independent shader code that manipulates UVs, and it might make sense in a real game to actually build a texture upside down intentionally so that UVs can be consistent between platforms.)
- Shaders
  - Add a texture coordinate as an input to each vertex shader, as an output to each vertex shader, and as an input to each fragment shader
    - Use a float2/vec2 for HLSL/GLSL
    - Use TEXCOORD0 for the HLSL semantic
    - Use location = 2 for the GLSL vertex shader input layout (to match what you did in the VertexAttrib code). You can use whatever location you want for the output/input between the vertex/fragment shader as long as it matches.
  - You should pass the texture coordinate that is input to the vertex shader unchanged to the fragment shader (just like you do with the vertex color)

## cTexture Class

- The location on disk that I chose for these files is something that we haven't seen before in this class
  - The header, cTexture.h, is in the usual location, as are the platform-specific cTexture.d3d.cpp and cTexture.gl.cpp files. You should add them to your solution like you should have been adding your other files.
  - The cTexture.cpp and Internal.h file, however, are in their own cTexture folder. This is a John-Paul coding style, and shouldn't be understood as the "correct' way of doing things, but rather just my current preference. Almost all of my classes have two files, an H and  CPP file. Some, however, end up needing more (either multiple CPP files if the functions get too long and complicated, or an "inline" file that I use so that I can keep the interface all in the header file but still #include implementations that need to be inlined (like templates, for example), etc.). In that case I make a separate folder an put everything besides the header file in it. You can decide whether to keep the structure like this or change it to something that you prefer.
- The Direct3D code has to use the D3D device and the D3D immediate context. I have indicated those with [D3DDEVICE] and [IMMEDIATECONTEXT]. You will have to update the code with your way of getting access to those.

## Sampler State

- There are different ways that a texture can be sampled, and a real game would have a mechanism for specifying different ways for different textures. In our class, however, we will just use a single way for all textures.
- You will need to set the sampler state when you initialize your Graphics system (i.e. in the same place where you create the Direct3D device or create the OpenGL rendering context).
- For Direct3D:
  - The sampler state will be represented by an ID3D11SamplerState*
  - You can create and set/bind it with the following code:

    ```cpp
    // Create a sampler state object
    {
      D3D11_SAMPLER_DESC samplerStateDescription;
      {
        // Linear filtering
        samplerStateDescription.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
        // If UVs go outside [0,1] wrap them around (so that textures can tile)
        samplerStateDescription.AddressU = samplerStateDescription.AddressV
          = samplerStateDescription.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;
        // Default values
        samplerStateDescription.MipLODBias = 0.0f;
        samplerStateDescription.MaxAnisotropy = 1;
        samplerStateDescription.ComparisonFunc = D3D11_COMPARISON_NEVER;
        samplerStateDescription.BorderColor[0] = samplerStateDescription.BorderColor[1]
          = samplerStateDescription.BorderColor[2] = samplerStateDescription.BorderColor[3] = 1.0f;
        samplerStateDescription.MinLOD = -FLT_MAX;
        samplerStateDescription.MaxLOD = FLT_MAX;
      }
      const HRESULT result = [D3DDEVICE]->CreateSamplerState( &samplerStateDescription, &[SAMPLERSTATE] );
      if ( FAILED( result ) )
      {
        EAE6320_ASSERT( false );
        eae6320::Logging::OutputError( "Direct3D failed to create a sampler state with HRESULT %#010x", result );
        return false;
      }
    }
    // Bind the sampler state
    {
      const unsigned int samplerStateRegister = 0; // This must match the SamplerState register defined in the shader
      const unsigned int samplerStateCount = 1;
      [IMMEDIATECONTEXT]->PSSetSamplers( samplerStateRegister, samplerStateCount, &[SAMPLERSTATE] );
    }
    ```

  - You release it like you do every other Direct3D object
- For OpenGL:
  - The sampler state will be represented by a GLuint ID
  - You can create and set/bind it with the following code:

    ```cpp
    // Create a sampler state object
    {
      const GLsizei samplerStateCount = 1;
      glGenSamplers( samplerStateCount, &[SAMPLERSTATEID] );
      const GLenum errorCode = glGetError();
      if ( errorCode == GL_NO_ERROR )
      {
        if ( [SAMPLERSTATEID] != 0 )
        {
          // Linear Filtering
          glSamplerParameteri( [SAMPLERSTATEID], GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR );
          EAE6320_ASSERT( glGetError() == GL_NO_ERROR );
          glSamplerParameteri( [SAMPLERSTATEID], GL_TEXTURE_MAG_FILTER, GL_LINEAR );
          EAE6320_ASSERT( glGetError() == GL_NO_ERROR );
          // If UVs go outside [0,1] wrap them around (so that textures can tile)
          glSamplerParameteri( [SAMPLERSTATEID], GL_TEXTURE_WRAP_S, GL_REPEAT );
          EAE6320_ASSERT( glGetError() == GL_NO_ERROR );
          glSamplerParameteri( [SAMPLERSTATEID], GL_TEXTURE_WRAP_T, GL_REPEAT );
          EAE6320_ASSERT( glGetError() == GL_NO_ERROR );
        }
        else
        {
          EAE6320_ASSERT( false );
          eae6320::Logging::OutputError( "OpenGL failed to create a sampler state" );
          return false;
        }
      }
      else
      {
        EAE6320_ASSERTF( false, reinterpret_cast<const char*>( gluErrorString( errorCode ) ) );
        eae6320::Logging::OutputError( "OpenGL failed to create a sampler state: %s",
          reinterpret_cast<const char*>( gluErrorString( errorCode ) ) );
        return false;
      }
    }
    // Bind the sampler state
    {
      // We will never be required to use more than one texture in an Effect in this class,
      // but it doesn't hurt to bind the state to a few extra texture units
      // just in case you decide to try using more
      const GLuint maxTextureUnitCountYouThinkYoullUse = 5;
      for ( GLuint i = 0; i < maxTextureUnitCountYouThinkYoullUse; ++i )
      {
        glBindSampler( i, [SAMPLERSTATEID] );
        const GLenum errorCode = glGetError();
        if ( errorCode != GL_NO_ERROR )
        {
          EAE6320_ASSERTF( false, reinterpret_cast<const char*>( gluErrorString( errorCode ) ) );
          eae6320::Logging::OutputError( "OpenGL failed to bind the sampler state to texture unit %u: %s",
            i, reinterpret_cast<const char*>( gluErrorString( errorCode ) ) );
          return false;
        }
      }
    }
    ```

- You release it with ```glDeleteSamplers()```

## Sampling a Texture in Shaders

- First you need to declare a texture object
  - In HLSL:
    - ```Texture2D someVariableName : register( t0 );```
  - In GLSL:
    - ```layout( binding = 0 ) uniform sampler2D someVariableName;```
  - Note that the zero in both is the register/ID that must be passed into cTexture::Bind(). To use more than one texture in a shader you would have to use different registers/IDs in their declarations.
  - If you completed the optional part of Assignment 12 you should be able to make a platform-independent macro for declaring texture objects. The tricky part is getting the "t" and number together in HLSL; you must use the ## preprocessor syntax. Here's an exampleLinks to an external site. that may be of help.
- In HLSL only you also must declare a sampler state object
  - The syntax is:
    - SamplerState someVariableName : register( s0 );
  - Note that the zero is what is passed to IDD3D11DeviceContext::PSSetSamplers() in the I gave above for setting/binding the sampler state. If you wanted to use more than one sampler state in a shader you would need to use different registers in their declarations and then pass the appropriate number when setting each of them.
  - If you completed the optional part of Assignment 12 you should be able to make a platform-independent macro for declaring sampler state objects. You will have to use the same ## concatenation trick for HLSL that you used for declaring texture objects. For GLSL you must #define the macro so that the compiler doesn't complain, but the trick is to #define it as nothing.
- To actually sample a texture you need 1) the texture object, 2) the texture coordinates of where you want to sample, and 3) the sampler state (in HLSL only).
  - In HLSL:
    - ```float4 sampledColor = someTextureObject.Sample( someSamplerState, someTextureCoordinates );```
  - In GLSL:
    - ```vec4 sampledColor = texture2D( someTextureObject, someTextureCoordinates );```
  - Two things that may not be obvious about these statements if you are new to graphics:
    - The "someTextureCoordinates" variable must be a float2/vec2 (which are "uv" coordinates, since this is sampling a 2D texture)
    - These functions will always return a float4/vec4 RGBA color
      - If you only want the color part of a texture sample you can add .rgb to the end of the sample function call (or .a if you only want the alpha part)
  - If you completed the optional part of Assignment 12 you should be able to make a platform-independent macro for sampling textures. The macro will have to take all three input parameters, but the GLSL version can ignore the sampler state variable name.
- To use the sampled color use multiplication
  - This means for a standard fragment shader the final color will be a result of three colors being multiplied together:
    - ```finalColor = colorFromVertex * colorFromConstantBuffer * colorFromTexture;```
  - In most cases for your final game you will only want to see the texture, and will not use vertex color or the constant buffer color. This means that they should both be white (because white is (1,1,1) and 1 * x = x). The MayaMeshExporter fills in white if no color set exists, and your Assignment 12 should set the g_color constant as white if it's not specified in the human-readable file.
  - If you want to try creating variations of the standard fragment shader (e.g. adding color animation) you should feel free to experiment! You will probably get the best results by combining the three input colors first (as shown above) and then changing that single color in some way.

## Authoring Images

- You are not required and I am not expecting you to do anything to make textures in this class besides finding free ones and downloading them (or taking your own photos and using them). It is possible, however, that you may find yourself in a situation where you would like to make some simple edits to something you've found to make it work better for your game.
- Photoshop is the gold standard, and if you have access to this and know how to use it is a good choice.
- An excellent free alternative, however, is Paint.NETLinks to an external site.. It is much easier for a beginner to figure out than Photoshop is and it can do anything that you might want to for our class. I use it rather than Photoshop if I just want to look at an image or do simple things.
- GIMPLinks to an external site. is another free alternative that is more fully-featured. Like Photoshop it is harder to learn how to use.

## Example Scene

- This is a screenshot from my reference implementation:
- assignment13_screenShot.png
- You should plan for your final game to have recognizable shapes and textures like this (as opposed to abstract cubes and spheres from Maya with random vertex colors). The scene above contains:
  - A free crocodile model that I got hereLinks to an external site.. It included the texture. I had to scale it down.
  - A free tree model that I got hereLinks to an external site.. It included the texture. I had to scale it down.
    - (I use a specialized vertex shader for the trees that does sine wave animation so it kind of looks like the wind is blowing)
  - A plane that I made in Maya (I adjusted the UVs so that the texture would tile, but this wasn't necessary and Maya will create good ones by default). It uses a free texture that I got hereLinks to an external site..
- The background isn't a texture; I just changed the clear color from black to a sky blue.
- I also changed the resolution so that it's no longer square, and I changed the camera's field of view. You should definitely feel free to change these yourself to make something more appropriate for your game.
- I don't expect your final game to have mind-blowing art, but I do expect you to put in the time to make it look nice. Regardless of your artistic talent or experience with Maya you can find free models and textures online and with a little help from Google learn how to scale them to fit into your scene.
  - (It's ok to have some basic shapes from Maya like my plane for the ground. If a basic shape will do the job then use it! Most of your objects, though, should be more interesting to look at. If you have questions or concerns about this then you should ask me.)