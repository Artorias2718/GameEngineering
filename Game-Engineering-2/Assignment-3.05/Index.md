---
---

# Assignment-3.05: Constant/Uniform Buffers

## Requirements

- Create a platform-independent struct declaration for the two kinds of constant buffer data (frame and draw call)
- Create a representation of a constant buffer
  - The interface must be platform-independent
    - There should be a way of specifying what kind of constant buffer it is (e.g. for per-frame or per-draw call data for this assignment)
    - There should be a way of initializing it with optional data (passed in as a void*)
    - There should be a way of binding/setting it (making it active on the GPU)
    - There should be a way of updating it (changing the data values)
  - The data should be cleaned up properly
- Convert the frame data in the Graphics.[platform].cpp files to use your constant buffer interface
  - If you do this requirement correctly then the code in both files dealing with frame data should be identical
- Add draw call data
  - Use the provided vertex shaders Download provided vertex shaders(don't use any vertex animation for this assignment)
    - (You will have to update your fragment shaders so that the name of the frame data constant buffers are the same as they are in the provided vertex shaders)
- Create a draw call constant buffer, initialize it, and bind it
- Update the submission function so that it accepts a mesh and two floats (representing the x and y screen coordinates of where that mesh should be drawn)
  - To update the existing square your game should pass in 0 and 0 for the X and Y screen position
- Instead of storing a list of meshes to be drawn each frame, store a list of items required to make draw calls. For this assignment each item should consist of:
  - A mesh (this should be a pointer or reference)
  - A draw call data struct (this should be an actual copy of the struct, constructed from the data the caller submits)
- When your draw/render function iterates through each submitted draw call, it should:
  - Update the draw call constant buffer with the data from the struct
  - Make the draw call by drawing the mesh
- Change your square mesh so that its vertices are centered around the local origin
- Create a new mesh and add an object that uses it to your game
  - The new mesh must have a different number of triangles than the square. (A single triangle is fine, but you can also have three or more if you prefer.)
  - The new mesh must be centered around the local origin
  - The object using it must have a different screen position than (0, 0) (i.e. so that it doesn't overlap the square)
- Allow the user to move the square around the screen dynamically by pressing keys
  - It must move independent of framerate
  - It should not move too fast or too slow. Make it feel "good" to move around the screen.
- Your write-up should:
  - Show a screenshot of your game running. Both the square and your new mesh should be visible.
  - Tell us what keys to use to move the square
    - (You will have to explain the controls on every future write-up, but since this is the first assignment with controls this is a courtesy reminder)
  - Show us the interface of your constant buffer representation (i.e. show us the function signatures of your initialization, binding/setting, and updating functions). It is ok to just copy/paste this from your header file as long as you only show interface and not implementation.
    - Explain how you allow a user to specify the type of constant buffer symbolically, and how you use that to set the appropriate location (register or binding point) that corresponds to the values in the shader code
    - Show us an example of the user specifying the type of constant buffer symbolically. If there is a 0 or a 1 in your example then your assignment isn't correct!
  - Tell us when and how your constant buffers are cleaned up
    - (Specifically, how did you ensure that they get cleaned up before the API objects themselves?)
  - Show us an example from your game code of submitting a draw call to the graphics system to be rendered
  - Show us your code in your render/draw function that iterates through each submitted draw call (and updates the draw call constant buffer and draws the mesh)

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- If you change the position of your second mesh in code to (1, 1) and build and run the game it should appear in the upper right of the screen. If you change it to (-1,-1) and build and run the game it should appear in the lower left of the screen.
  - (It should not be hard for Saurabh to figure out how to do this by looking at your game code. If it is and takes him a long time then you may lose points.)

## Details

### Constant Buffer Data

- The constant data that the CPU sends to the GPU should be platform-independent
  - (In practice this is not always possible, and often not even desirable (e.g. a newer platform might be able to support more advanced features than an older platform that require constant data), but for our class we will do it to make it easier to create a platform-independent graphics system interface.)
- You should declare the structs in a single platform-independent place
  - (You should already have a struct declaration for frame data in both Graphics.[platform].cpp files, so you will need to move this to a platform-independent place and add a new draw call data struct in that same place.)
- You can name the struct and its members anything that you like, but the data layout must be:

  - Frame data:

    ```cpp
    struct sFrame
    {
      union
      {
        float g_elapsedSecondCount_total;
        float register0[4];
      };
    };
    ```

  - Draw Call data:

    ```cpp
    struct sDrawCall
    {
      union
      {
        float g_objectPosition_screen[2];
        float register0[4];
      };
    };
    ```

- The array of four floats is to add the appropriate padding that the GPU expects

### Constant Buffers

- Platform-Specific Data
  - The only platform-specific data your representation should encapsulate is:
    - For Direct3D:
      - ID3D11Buffer*
    - For OpenGL:
      - GLuint
- Type
- In our class we will have three different kinds of constant buffers:
  - Frame data
    - This is data that can only change every frame. The data in the struct must be updated at the start of rendering each frame, and then that updated data must be sent to the GPU.
  - Draw Call data
    - This is data that changes with every draw call. The data in the struct must be updated before every draw call, and that updated data must be sent to the GPU before every draw call.
  - Material data
    - We haven't learned about materials yet, but there will be many different materials in a game and each one of these materials will have its own data. Unlike the other two kinds of constant buffers the material data won't change frequently (it will never change in our class), but because there are many different constant buffers a new one must be bound to the GPU before any draw calls using that specific material are made.
- You should only have one representation of a constant buffer (only one class/struct/whatever), but each instance of a constant buffer must be able to do the following based on which type it is:
  - Know which arbitrary ID (or "register") it is assigned to in our shader code
    - In our class this is 0 for frame data, 1 for draw call data, and 2 for material data
    - The interface must not rely on the user knowing these magic numbers when creating or initializing a constant buffer! You must allow the user to specify the type of constant buffer symbolically. (Or, put another way, you should be able to change the arbitrary IDs assigned to each type without having to change any code that uses your constant buffer representation.)
- Initialization
  - Your initialization function should accept as parameters:
    - The constant buffer's type
    - The size (in bytes) that needs to be allocated for the constant buffer
      - The caller will use sizeof( theCorrespondingStruct )
    - An optional void* of initial data to populate the constant buffer with
      - The caller will use &someCorrespondingStructInstance
      - You (as the caller) will provide initial frame data because you should know the total seconds that have elapsed. You will not provide any initial draw call data.
  - You may choose to have a platform-independent initialization function that sets common data (like the type and maybe the size if you decide to store it), which then calls a platform-specific helper function to do the rest
  - The platform-specific initialization will be almost exactly what is in the CreateConstantBuffer() functions that were provided in the Graphics.[platform].cpp files for Assignment 01. You should be able to figure out the few things that need to be changed.
    - Direct3D requires that the size of a constant buffer allocation is a multiple of 16. You should use the function in the provided Math library Download provided Math libraryto do this:

      ```cpp
      bufferDescription.ByteWidth = Math::RoundUpToMultiple_powerOf2( static_cast<unsigned int>( SIZE ), 16u );
      ```

- Binding/Setting
  - Once you bind or set a constant buffer on the GPU to a certain location ("register" or "binding point"), it will be available to all future shaders and draw calls at that location until you bind or set a different constant buffer to that location
  - This is done with VSSetConstantBuffers() / PSSetConstantBuffers() in Direct3D and with glBindBufferBase() in OpenGL. If you look for those function calls in Graphics.[platform].cpp you should be able to figure out how to do the same in your constant buffer representation.
  - Both platforms require you to tell them how to make the data accessible from the shader. In Direct3D you specify a register number and in OpenGL you specify a binding point. This is a mostly arbitrary number that is assigned in the shader source code. This number must come from the type of constant buffer that was assigned at initialization (see above for the discussion of this and which values to use). Be sure to specify the correct number here, or the API will not set your data in the correct spot for your shaders to use.
  - No extra input parameters should be required to bind or set a constant buffer besides your constant buffer representation itself
    - (Optional task: You may choose to require the caller to specify which shader types to bind the constant buffer to (i.e. vertex shader only, fragment shader only, or both), and then only set the corresponding data in Direct3D (OpenGL always sets the data for every shader type). This is more efficient but not required; only do it if you are interested. If you do implement this interface, then frame data should be set for both vertex and fragment shaders, but draw call data should only be set for vertex shaders.)
- Updating
  - This is how you tell Direct3D or OpenGL to change the data of a constant buffer (remember that the graphics APIs own the constant buffer memory, and so to change it you have to explicitly give them the new data that you want and let them handle it in their own way)
  - This is done with Map() / Unmap() in Direct3D and glBindBuffer() / glBufferSubData() in OpenGL. If you look for those function calls in Graphics.[platform].cpp you should be able to figure out how to do the same in your constant buffer representation.
  - This code needs the data as a void* and the size of the data, similar to your initialization function. The caller should always provide data with &someStructInstance, but you have a choice of how to figure out the size of the data. On initialization you had to tell the API how much space to allocate for your constant buffer, and so you may choose to store that value and always use it in your update function (and not require the caller to provide a size). This is the most convenient for the caller, although it requires him/her to always pass in the full struct (this will always be the case in our class). Alternatively, you could require the caller to pass in the size of the data. This is less convenient for the caller, but offers greater flexibility. It does mean, though, that a caller could pass in a greater size than was allocated, which would cause problems (in the best case the API would detect this and fail, but it might also cause a buffer overrun or crash). You could also take a hybrid approach and store the allocated size but still allow a caller to pass in a size that was less than or equal to the allocated size. You could then allow the caller to pass in less data if desired, but prevent him/her from ever passing in more by checking and failing.
- Clean Up
  - You should be able to figure out how to move the code from Graphics.[platform].cpp to your constant buffer representation
  - You will have to think a bit carefully about how to control when your constant buffers are cleaned up. If you put the clean up code in the destructor but keep the constant buffers as static instances in the Graphics.[platform].cpp file then you will probably run into trouble on exiting the program because the graphics APIs will be cleaned up before the constant buffers. There are two ways to fix this; can you figure out what you would need to do?

### Centering Around the Local Origin

- The local origin is (0, 0). To create a square centered around this point, every number in every position will have the same value, but some will be positive and some will be negative.
  - It is up to you how big the square is, but consider making it smaller rather than larger to give it room to move around the screen
- You should only have to change the position values to do this. The indices and colors can remain the same.

### Adding a Second Mesh

- You can copy your square mesh file as a starting point and then modify it
  - (The requirement to have it centered around the local origin doesn't have to be mathematically accurate. As long as it is generally on screen where its constant buffer draw call position says it should be it's ok.)
- You will have to build your new mesh file so that it's available to the game. Do you remember how to do this?

### Adding a Second Object to the Game

- We have talked almost exclusively about engine architecture, and not much about game architecture. You probably made lots of (reasonable) assumptions in order to draw the single square in past assignments, but will have to rethink them now that we are adding a second object. You will need two meshes and two positions to have two objects. Do you want to start trying to encapsulate an "object" into some kind of struct or class, or do you want to keep the data separate until you have a better idea of what else we might be adding in future assignments? Both are reasonable choices. If you start adding more structure now it may save you time in the future as your game gets more complicated, but it might also be a waste of time if you add a lot of code and end up having to redo it. If you don't add any structure now and do the minimum it will definitely save you time for this assignment, but you may regret it in future assignments as things get more and more complicated.
- Here is an example of what your game could look like with a triangle as the second object (you don't have to match this exactly! Feel free to be creative with shapes, sizes, positions, and colors):
- assignment05_screenshot.png

### Moving the Square

- I would prefer you to use code you have from JBarnes's class to move the square, but I have also provided a very basic UserInput project Download provided a very basic UserInput projectthat you can use. A basic example of using the example project to calculate a framerate-independent XY offset using the arrow keys be:

  ```cpp
  float offset[2] = { 0.0f, 0.0f };
  {
    if ( UserInput::IsKeyPressed( VK_UP ) )
      offset[1] += 1.0f;
    if ( UserInput::IsKeyPressed( VK_DOWN ) )
      offset[1] -= 1.0f;
    if ( UserInput::IsKeyPressed( VK_LEFT ) )
      offset[0] -= 1.0f;
    if ( UserInput::IsKeyPressed( VK_RIGHT ) )
      offset[0] += 1.0f;
  }
  {
    const float speed_unitsPerSecond = 0.1f;
    const float offsetModifier = speed_unitsPerSecond * Time::GetElapsedSecondCount_duringPreviousFrame();
    offset[0] *= offsetModifier;
    offset[1] *= offsetModifier;
  }
  ```

- The logic for moving the square should happen in your game project as part of your gameplay logic, and should happen before the objects are submitted to be drawn. (The state of the game should be updated, and then everything based on that update anything that should be displayed that frame should be submitted to the graphics system to be rendered.) You should be able to figure out how to do this.