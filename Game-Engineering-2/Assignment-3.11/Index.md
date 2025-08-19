# Assignment 3.11: Material Files

## Requirements

- Create a new struct for material constant data
  - You can name things whatever you'd like, but the layout should be:

    ```cpp
    struct sMaterial
    {
      struct
      {
       float r, g, b, a;
      } g_color;
    };
    ```

  - Add a default constructor that sets each float to 1.0f (making the color white and fully opaque)
- Update your shaders to use material constant buffers
  - You will have to add a declaration for the material constant buffer
    - The layout must match the struct shown above. What type should g_color be?
    - The ID (register) must be 2
    - You only need to add the material constant buffer declaration to fragment shaders for our class
- You should modify the color in your fragment shaders by the material constant color
  - This means that you multiply g_color by the input vertex color
  - (Note that if you do this before you set values and bind the constant buffer in C code that g_color will probably default to black (zeros), and you won't see anything when you run your game.)
- Add a representation of a material to your run-time code
  - A material consists of:
    - An effect
    - A constant buffer
  - A material should have functions that do the following:
  - Load
    - This function takes the path of a material file as input and loads the binary data
    - It extracts a path of an effect and uses it to load the effect
    - It extracts the constant data and uses it to initialize the constant buffer
      - (Note that a material's data will never change, and so once the constant buffer is initialized the struct doesn't need to be kept around)
  - Clean Up
  - Bind
    - This should bind the constant buffer
    - This should bind the effect
  - Your material (both interface and implementation) should be completely platform-independent
    - All of the platform-specific code should be hidden in your effect and constant buffer representations
- Update your graphics submission code to use a material instead of an effect
  - The game should now care about which material a mesh is rendered with. It will no longer know about or care about effects. (An artist will create a material with a certain effect, but a gameplay programmer just cares about the material that a thing is made out of.)
- Create a human-readable material file format
  - A path to an effect is required
  - Any constant data, however, should be optional
- Add an asset type info for materials
  - Remember that materials reference effects
- Create a MaterialBuilder tool
  - There are two things that a binary material needs:
    - Constant data
    - The path to the effect
  - You should output the constant data struct with a single write operation
- Create two default materials for your two effects
  - These should use the default white for g_color, and they should be used for any existing objects in your game
- Create two new materials that use different colors, and render the same mesh with each of them
  - In other words, you must render one mesh with two different colors. There should be two separate objects with two different colors that use the same mesh. (See the example screenshot of the horses below.)
  - If you can think of a way to do this that you can really use for your game then please do! (Next week we will add textures instead of just single colors, if that helps you think of a good way to use different materials.) If you can't think of any good way, though, then you can just make a random shape in Maya for your mesh and put two of them in your game with different colors temporarily for this assignment.
- Your write-up should
  - Show us an example of your human-readable material file format with g_color set to something besides white
  - Show us the binary file of that same material (a screenshot from a hex editor is ok as long as it's readable)
    - Identify the constant data struct in the binary file
- Show a screenshot of your game that shows the two differently-colored materials
- Tell us what progress you have made on your final project game
  - What changes have you made since Assignment 09?
  - What is the current status of the game?
  - Have any of your plans changed since Assignment 09?

## Optional

- Make your shaders more platform-independent
  - I have provided a shaders.inc Download shaders.incfile that you can use as a starting point
  - Every shader file should #include this file before doing anything else
    - (Remember that the path passed to the #include directive must be relative to the file doing the including)
  - The provided file does four things:
    - Defines the GLSL version
      - (You will have to remove the #version directive from every shader once you #include shaders.inc)
    - Defines types that are different between HLSL and GLSL
      - That means, for example, that you can use "float4" in GLSL or "vec4" in HLSL
    - Defines macros for declaring constant buffers
      - This gives a platform-independent interface using the preprocessor
    - Defines the the frame constant buffer using the macro
      - (You will have to remove the frame constant buffer declaration from every shader once you #include shaders.inc)
- If you want to use shaders.inc you must do the following:
  - Implement the ShouldTargetBeBuilt() function for shaders in AssetBuildSystem.lua so that if shaders.inc ever changes the shaders will also be built again. The implementation will look something like this:

    ```lua
    ShouldTargetBeBuilt = function( i_lastWriteTime_builtAsset )
      local lastWriteTime_includeFile = GetLastWriteTime( s_AuthoredAssetDir .. "Shaders/shaders.inc" )
      return EAE6320_TODO: Has shaders.inc been modified more recently than the asset?
    end
    ```

- Once shaders.inc is working you should consider the following enhancements:
  - Declare the draw call constant buffer and the material constant buffer
    - If you do that you won't have to update every shader file if any constant ever changes
- Define a macro to transform a vector by a matrix
  - HLSL uses a mul() function and GLSL uses the * operator. Can you figure out a way to define a macro to give the two a common interface?
- Make the entire main function's contents platform-independent
  - You will probably need to keep the input and output declarations platform-specific, but you should be able to make the actual implementation of main() platform-independent so that you only have to write it one time.
  - (One exception is that GLSL requires the output vertex position variable to be called gl_Position. There are various ways around this, but the easiest is to just make an #if/#else statement for the variable name.)

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- If you build the solution, change the color in one of your human-readable material files, build the solution again and run the game, then an object should have the new color that was set in the material

## Details

### Human-Readable Material File

- For Assignment 12 we only have a single piece of constant data, which is g_color. That means that if a material file specifies data for g_color it will be used, but if not the default (white and fully opaque) should be used instead.
- In the future you may add more constant data for materials, but a given human-readable material file only needs to specify the ones that it cares about. With this in mind, how should constant data be defined in Lua?
- When you read in the Lua file in your MaterialBuilder it is ok and expected that you hard-code the constants that you are looking for. In a real game things would be much more complicated and data-driven, but my expectation is that your code will specifically look for "g_color" and, if it finds it, know how to interpret the data in order to fill in the "g_color" member of the constant data struct. DON'T MAKE THIS ANY MORE COMPLICATED! (It's a fun problem to solve and you may want to give it a try in the future, but don't do anything fancy for this assignment.)
  - This means that, for our class, every time you add a new material constant to the material constant struct you will have to update your MaterialBuilder tool to explicitly look for it in the human-readable material files
- When parsing the Lua constant data your MaterialBuilder should be populating a material constant data struct. That means that any data that is in the file will be put in the struct, and if any data is not in the file default values will be used. (This is why you add a default constructor to the material data struct.)

### Binary Material File

- You already know how to write a path to a binary file, but now you will also have fixed-length binary data. Should the effect path go first and the constant data second, or the constant data first and the effect path second? Why? (One way is better than the other.)

### One Mesh with Different Colors

- Here is an example of a single horse mesh being rendered with a yellow material and with a purple material:
- assignment12_horseOfADifferentColor.png