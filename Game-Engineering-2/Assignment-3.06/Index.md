# Assignment 3.06 - 3D Meshes

## Requirements

- Enable backface culling in OpenGL
- Add a third dimension to your mesh format
- Add the vector, quaternion and transformation matrix files to your math project
  - (Technically this isn't required, but I believe that you will have trouble getting the transformations to work in both platforms if you use your own. If you really want to try using your own my advice would be to start with the files provided, get the assignment working, and only then try to switch to your own.)
- Update your constant buffers
  - The layout for your frame data should be this (you can name the data anything that you want):

  ```cpp
  struct sFrame
  {
    Math::cMatrix_transformation g_transform_worldToCamera;
    Math::cMatrix_transformation g_transform_cameraToScreen;
    union
    {
      float g_elapsedSecondCount_total;
      float register8[4];
    };
  };
  ```

- The layout for your draw call data should be this (you can name the data anything that you want):

  ```cpp
  struct sDrawCall
  {
    Math::cMatrix_transformation g_transform_localToWorld;
  };
  ```

- Update how your draw calls are submitted and stored
- Create a representation of a 3D camera
  - All of the camera code (interface and implementation) must be platform-independent
  - A camera needs the following data:
    - A position (use a Math::cVector)
    - An orientation (use a Math::cQuaternion)
    - A field of view (use a float)
    - A near plane distance and a far plane distance (use floats)
    - An aspect ratio
      - This is width / height. In our class this should always be the screen resolution width divided by the screen resolution height. You do not need to store the aspect ratio with the camera if you have some other way of querying or calculating the aspect ratio when it is needed.
  - Your camera needs to be able to do the following things (or, put another way, your game and graphics project needs to be able to do the following things given a camera):
    - Move (change its position)
    - Rotate (change its orientation)
    - Calculate a world-to-camera transformation matrix (use Math::cMatrix_transform's function)
    - Calculate a camera-to-screen transformation matrix (use Math::cMatrix_transform's function)
- Add a way to submit a camera for the current frame
- Your camera must be able to be moved forwards and backwards, side-to-side, and up and down using the keyboard
  - The camera should move at a "good" speed. Make it easy to move around, but not too fast.
  - (You are not required to add any other camera controls, but this doesn't mean that you shouldn't. If there are any other controls that you think could help you during development then don't hesitate to add them.)
- Draw the following two objects:
  - A unit cube
    - Each position coordinate should be either 1 or -1
    - Each vertex should have a different color than any of its adjacent vertices
      - (It's ok to repeat colors if you want to, but if you do they must be at opposite corners)
  - A plane that serves as a ground/floor for the cube
    - The Y coordinate of each vertex should be 0. The X and Z coordinates should be +n or -n, where "n" is chosen by you to look good.
    - The plane and its triangles should be oriented so that it is visible when the camera is above the floor. If the camera moves below the floor it must no longer be visible.
    - You may choose any colors for the floor that you want except for black (it is ok if you want them to all be the same color)
- When the game starts the cube must be "resting" on the floor and not intersecting it
  - This means that you will either have to move the cube object's position up or the plane object's position down.
- When the game starts the camera should be above the cube
  - You should choose a view that looks "good". The player should be able to easily see the cube and floor before doing anything, and should be able to move the camera and cube
- The cube must be able to be moved left-to-right and up and down with the keyboard
  - The cube should move at a "good" speed. It should be easy to move around, but not too fast.
  - (You may also add other controls if you wish.)
- The cube must rotate around the Y axis with a constant framerate-independent speed
  - It should rotate at a "good" speed. It should be fast enough that the player doesn't have to wait a long time to see that the back side of the cube is drawn correctly, but slow enough that it's easy to tell that it's a cube rotating and to see the different colors at each vertex.
- Enable depth buffering:
  - Create a depth buffer
  - Clear the depth buffer to 1.0f every frame
  - Enable depth testing and depth comparisons in OpenGL (it is enabled by default in Direct3D)
- Your write-up should:
  - Show a screenshot of your colorful cube on top of your floor when the game starts up (it's ok if the cube has rotated)
  - Show us a second screenshot that:
    - Shows that the camera can move (either bring it in close or far away)
    - Shows the cube intersecting with the floor (to show that the cube can move and that depth buffering is working).
      - ("Intersecting" means that it should be moved down into the floor, with part of it above and part of it below)
  - Tell us how many vertices and triangles a cube requires when using indexed drawing with a triangle list (like we are doing for this assignment)
    - Compare that to how many vertices and triangles a cube would require if we just had a vertex buffer but no index buffer (like we did when we started the class)
    - Tell us how much memory is saved with the current sVertex struct by using indexed drawing for the cube? (Explain how you came up with your answer. Don't forget to include the memory required by the indices.)
      - In other words, what is the difference in bytes between the memory that the cube's vertices and indices require using indexed drawing and the memory that the cube's vertices would require without indexed drawing?
  - Show us the interface for your camera (the header file)
    - Explain why you chose your interface and how it makes it easy for a gameplay programmer to control a camera
  - Show us the game code that submits a draw call (the code in your game and not in the graphics project)
    - Tell us how that data gets transformed into the draw call constant buffer data
    - Tell us what data you store in the list of submitted draw calls
- Show us how the gameplay programmer deals with submitting the camera
  - Tell us why you think this is the most convenient way for a gameplay programmer to deal with submitting a camera

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- You should only see the floor when you're above it and not when you move the camera below it
- You should be able to see each face of the cube if it is facing the camera (including the back face when it rotates into view and the bottom face when the cube is above the camera)

## Details

### Backface Culling

- Enable this by calling:
  - ```glEnable( GL_CULL_FACE );```
- This should happen in Graphics.gl.cpp, in Initialize()
- It must happen after the rendering context has been created
- You can add error checking. You should know how to check for OpenGL errors by now (you can look at other function calls in the same file if you need a reminder).
- My recommendation is to do this before you do anything else in the assignment and make sure that your triangles still render in OpenGL. If they don't it means that your winding order is incorrect and you should fix it.

### Mesh Changes

- You will have to add a third number to the positions in your Lua file
  - My advice would be to start with the exact same square you had in Assignment 05, but:
    - Make each X and Y dimension a unit length (i.e. either 1 or -1)
    - Make each Z dimension a 1
  - I would only do this to start the assignment, and try to get this square working in 3D before adding the rest of the cube
- You will have to update the sVertex struct in your run-time code to add another Z float
  - You may have to force a rebuild (not just a standard incremental build) after making this change
    - (This may be a compiler bug, although if anyone figures out something wrong with our solutions that makes the change not update properly in an incremental build please let me know!)
- You will have to update the Lua loading code to read in the new Z position into the struct
- You will have to update the vertex format for both platforms
  - The conceptual change to both platforms is the same: Instead of two floats for position you now have three. The API details of how to make this change are different, however. Can you look at the existing code and figure out what to change for each platform?
  - (Just to give you an idea of the scope of the required change, I had to only change one line for each platform in my reference implementation (not counting updating the comments about the size and offsets).)
  - You will have to change the two vertex shaders to accept a 3-dimensional position. You should just use the shaders that I provide.

### Constant Buffers

- You will have to change all four shaders to use the new constant buffers. You should just use the vertex shaders that I provide, but you will have to update the frame constant buffers in the fragment shaders. Note that the order of the data matters, and that the constant buffer definitions in the fragment shaders should be identical to their corresponding definition in the vertex shaders.

### Submitting Draw Calls

- The data that the draw call constant buffer stores is not the same as the data that your game should store for each object
  - Each 3D object that your game keeps track of (the game and not the graphics engine) should have:
    - A position, which is a Math::cVector
    - An orientation, which is a Math::cQuaternion
  - What data should the game submit? (Given that we want to make life simple for a gameplay programmer.)
  - How can that data be used to get the data that the draw call constant buffer needs?

### Camera

- The field of view can either be horizontal or vertical
  - My advice for our class is to store vertical, and to use 60 degrees while trying to get this assignment working
    - (You can change this number once everything is working if you want to)
  - The code in Math::cMatrix_transform that creates the camera-to-screen transformation expects vertical field of view. If you use horizontal for your camera you will have to convert it.
  - The code in Math::cMatrix_transform that creates the camera-to-screen transformation expects the field of view in radians. If you want to use 60 degrees you will have to convert from degrees to radians or you will have problems. The Math project has a function to convert degrees to radians that you can use.
- In general the range of the near and far plane distances should be as small as possible (i.e. they should be as close to each other as you can get away with without clipping any near or far geometry) because that results in the maximum possible precision. In practice, though, there are many considerations to worry about that we won't discuss in our class.
  - My advice for our class would be to use 0.1 for the near plane distance and 100 for the far plane distance
  - In most real games you will need a larger distance than 100, but in our class this will be fine for the early assignments, and will probably be fine for most of your final projects
- In order to help check your work, the following screenshot was made with the following data:
  - The camera's orientation hasn't been changed (it is using the default Math::cQuaternion constructor which means it is facing the negative Z direction in a right-handed coordinate system)
  - The camera's position is (0, 0, 10) (it is moved 10 units in the Z direction but still facing the world origin)
  - The vertical field of view is 60 degrees
  - The near plane is 0.1 and the far plane is 100
  - The object is a unit square with +1 for its Z coordinates (it is a square that will become the front face of a cube)
- assignment06_square.png
- When you start your assignment I would recommend trying to duplicate the above screenshot in both platforms (except for the vertex colors, which can be anything you want) as a first step. (This is the most challenging part of the assignment because of all of the changes required and various things that can go wrong. If you can draw a unit square successfully then it is easier to do each of the remaining requirements one at a time.)

### Submitting a Camera

- Every frame must be drawn with a specific camera. We will only ever use one in our class for assignment requirements, but some of you may choose to be able to switch to different cameras for your final project games (you may want to switch to a different close-up view to show a player death, for example, and then switch back to standard world camera).
- Since there will only be one camera for a frame you don't need to keep a list of submitted ones like you do with draw calls. The game code will have a camera that it controls (moves and orients), and then it will submit that camera to be drawn.
  - You should think of a good interface for managing the camera in your game code. Do you want to make the gameplay programmer submit it every frame? Or maybe the gameplay programmer would prefer to just set a current camera, and then there would be a different function called automatically every frame to submit whatever the current camera is?
  - You should figure out what happens in the graphics system when a camera is submitted. How does the appropriate constant buffer data get set from the camera object that gets submitted?

### Making a Cube

- If you know how to make a square it is not hard to understand how to make a cube, but you may find that actually doing it is more difficult than you expect. My advice is:
  - Create the front face first
  - Add the vertices for the back face by copying/pasting the ones for the front face but changing the Z coordinates to -1
  - Only add indices to create one additional face at a time and make sure that it works before moving on to the next one
    - For example, after getting the front face working you may choose to add the top face next. After that is working (and only after) you may decide to add the left face, and after that is working the right face, and so on.
    - Being able to move the cube and the camera will really help with this part, so I would make sure that works before adding any faces other than the front one
- Please consider drawing a picture to help you create the cube! A few of you may be able to easily do it in your head, but if you have never worked with graphics before it will make things much easier to get a pencil and paper and actually draw out the vertices and triangles to figure out what you should add to your mesh file.

### Rotating the Cube

- The code that does this should be in your game code, and should be similar to the code that moves the cube except that it happens automatically instead of just when a key is pressed
- Notice that Math::cQuaternion has a constructor that accepts an angle (in radians) and a (normalized) axis. The easiest way to satisfy this requirement is to keep track of the angle yourself and construct a new quaternion for the cube's orientation every frame that you will submit:
  - Initialize the angle to 0 when the game starts
  - Decide on a rotational speed
  - Every frame increase the angle based on how much time has passed and the desired rotational speed
  - Use that angle and the Y axis (0, 1, 0) to construct a quaternion for the cube's orientation

### Depth Buffering

#### Creating a Depth Buffer

- In Direct3D:
  - You will need to add an ID3D11DepthStencilView* (in the same place where you currently have an ID3D11RenderTargetView*)
  - Make sure to initialize it to NULL and clean it up if it's non-NULL just like the render target view
  - The following function shows how I have modified my code that creates the render target view to also create the depth stencil view. You should be able to use it to modify your own code.

    ```cpp  
    bool CreateViews( const unsigned int i_resolutionWidth, const unsigned int i_resolutionHeight )
    {
      bool wereThereErrors = false;
    
      ID3D11Texture2D* backBuffer = NULL;
      ID3D11Texture2D* depthBuffer = NULL;
    
      // Create a "render target view" of the back buffer
      // (the back buffer was already created by the call to D3D11CreateDeviceAndSwapChain(),
      // but we need a "view" of it to use as a "render target",
      // meaning a texture that the GPU can render to)
      {
        // Get the back buffer from the swap chain
        {
           const unsigned int bufferIndex = 0; // This must be 0 since the swap chain is discarded
           const HRESULT result = [SWAPCHAIN]->GetBuffer( bufferIndex, __uuidof( ID3D11Texture2D ), reinterpret_cast<void**>( &backBuffer ) );
           if ( FAILED( result ) )
           {
             EAE6320_ASSERT( false );
             eae6320::Logging::OutputError( "Direct3D failed to get the back buffer from the swap chain with HRESULT %#010x", result );
             goto OnExit;
           }
        }
        // Create the view
        {
          const D3D11_RENDER_TARGET_VIEW_DESC* const accessAllSubResources = NULL;
          const HRESULT result = [DEVICE]->CreateRenderTargetView( backBuffer, accessAllSubResources, &[RENDERTARGETVIEW] );
          if ( FAILED( result ) )
          {
            EAE6320_ASSERT( false );
            eae6320::Logging::OutputError( "Direct3D failed to create the render target view with HRESULT %#010x", result );
            goto OnExit;
          }
        }
      }
      // Create a depth/stencil buffer and a view of it
      {
        // Unlike the back buffer no depth/stencil buffer exists until and unless we create it
        {
          D3D11_TEXTURE2D_DESC textureDescription = { 0 };
          {
            textureDescription.Width = i_resolutionWidth;
            textureDescription.Height = i_resolutionHeight;
            textureDescription.MipLevels = 1; // A depth buffer has no MIP maps
            textureDescription.ArraySize = 1;
            textureDescription.Format = DXGI_FORMAT_D24_UNORM_S8_UINT; // 24 bits for depth and 8 bits for stencil
            {
              DXGI_SAMPLE_DESC& sampleDescription = textureDescription.SampleDesc;
              sampleDescription.Count = 1; // No multisampling
              sampleDescription.Quality = 0; // Doesn't matter when Count is 1
            }
            textureDescription.Usage = D3D11_USAGE_DEFAULT; // Allows the GPU to write to it
            textureDescription.BindFlags = D3D11_BIND_DEPTH_STENCIL;
            textureDescription.CPUAccessFlags = 0; // CPU doesn't need access
            textureDescription.MiscFlags = 0;
          }
          // The GPU renders to the depth/stencil buffer and so there is no initial data
          // (like there would be with a traditional texture loaded from disk)
          const D3D11_SUBRESOURCE_DATA* const noInitialData = NULL;
          const HRESULT result = [DEVICE]->CreateTexture2D( &textureDescription, noInitialData, &depthBuffer );
          if ( FAILED( result ) )
          {
            EAE6320_ASSERT( false );
            eae6320::Logging::OutputError( "Direct3D failed to create the depth buffer resource with HRESULT %#010x", result );
            goto OnExit;
          }
        }
        // Create the view
        {
          const D3D11_DEPTH_STENCIL_VIEW_DESC* const noSubResources = NULL;
          const HRESULT result = [DEVICE]->CreateDepthStencilView( depthBuffer, noSubResources, &[DEPTHSTENCILVIEW] );
          if ( FAILED( result ) )
          {
            EAE6320_ASSERT( false );
            eae6320::Logging::OutputError( "Direct3D failed to create the depth stencil view with HRESULT %#010x", result );
            goto OnExit;
          }
        }
      }
    
      // Bind the views
      {
        const unsigned int renderTargetCount = 1;
        [IMMEDIATECONTEXT]->OMSetRenderTargets( renderTargetCount, &[RENDERTARGETVIEW], [DEPTHSTENCILVIEW] );
      }
      // Specify that the entire render target should be visible
      {
        D3D11_VIEWPORT viewPort = { 0 };
        viewPort.TopLeftX = viewPort.TopLeftY = 0.0f;
        viewPort.Width = static_cast<float>( i_resolutionWidth );
        viewPort.Height = static_cast<float>( i_resolutionHeight );
        viewPort.MinDepth = 0.0f;
        viewPort.MaxDepth = 1.0f;
        const unsigned int viewPortCount = 1;
        [IMMEDIATECONTEXT]->RSSetViewports( viewPortCount, &viewPort );
      }
    
    OnExit:
    
      if ( backBuffer )
      {
        backBuffer->Release();
        backBuffer = NULL;
      }
      if ( depthBuffer )
      {
        depthBuffer->Release();
        depthBuffer= NULL;
      }
    
      return !wereThereErrors;
    }
    ```

- In OpenGL:
- The desired attributes that are passed to wglChoosePixelFormatARB() need the following two key/value pairs added:

  ```cpp
  WGL_DEPTH_BITS_ARB, 24,
  WGL_STENCIL_BITS_ARB, 8,
  ```

- The PIXELFORMATDESCRIPTOR that is passed to SetPixelFormat() needs the following two fields set:

  ```cpp
  pixelFormatDescriptor.cDepthBits = 24;
  pixelFormatDescriptor.cStencilBits = 8;
  ```

### Clearing the Depth Buffer
- Both platforms need to clear the depth buffer to 1.0f (just like both platforms clear the color back buffer to black)
- In Direct3D:

  ```cpp
  const float depthToClear = 1.0f;
  const uint8_t stencilToClear = 0; // Arbitrary until stencil is used
  [IMMEDIATECONTEXT]->ClearDepthStencilView( [DEPTHSTENCILVIEW], D3D11_CLEAR_DEPTH,
    depthToClear, stencilToClear );
  ```  

- In OpenGL:

  ```cpp
  // Black is usually used
  glClearColor( 0.0f, 0.0f, 0.0f, 1.0f );
  EAE6320_ASSERT( glGetError() == GL_NO_ERROR );
  glDepthMask( GL_TRUE );
  // Clear depth to 1
  glClearDepth( 1.0f );
  EAE6320_ASSERT( glGetError() == GL_NO_ERROR );
  const GLbitfield clearColorAndDepth = GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT;
  glClear( clearColorAndDepth );
  EAE6320_ASSERT( glGetError() == GL_NO_ERROR );
  Enabling Depth Buffering
  This only needs to be done in OpenGL (it is enabled by default in Direct3D). You should put the code in the same place where you enabled culling in OpenGL (where the graphics system is being initialized, after the rendering context has been initialized):
  // Depth Testing
  glDepthFunc( GL_LESS );
  glEnable( GL_DEPTH_TEST );
  // Depth Writing
  glDepthMask( GL_TRUE );
  ```

- As with culling, you can add error checking after each function call
- Below is an example of what a finished assignment could look like:
- assignment06_finished.png