---
---

# Assignment 3.04 - Vertex Color & Indexed Drawing

## Requirements

- Change your mesh representation to use indexed drawing
  - Each of your meshes must store the number of vertices in its vertex buffer as a uint16_t and the number of triangle indices in its index buffer as a uint16_t
  - The platform-independent interface Draw/Render function used in Graphics.[platform].cpp shouldn't change
- Create a mesh file format
  - It must use Lua, and it must follow the format defined and described in the LoadTableFromFile section of the Using Lua example
  - It must be human-readable (see the Details section for more information about what this means)
- Create a file of your square mesh, and load it at run-time rather than hard-coding the data
  - There must be only one platform-independent mesh file that both platforms load (an artist is not going to make a different mesh for every platform)
- Add a color to each vertex
  - Each channel (i.e. red, green, blue, or alpha) must be represented as a normalized [0,1] float in your Lua file, but as a [0,255] uint8_t at run-time
  - Each vertex of your square must be a different color
- For this assignment you must disable the color animation in your fragment shader so that it is easy to see the different per-vertex colors)
- Your write-up should:
  - Show a screenshot of your game running. There should be a colorful square (possibly distorted due to vertex position animation), and it must be obvious that each vertex in the square has a different color.
  - Tell us the (minimum) number of vertices required to draw a square when using indexed drawing, Tell us the number of indices required.
  - Tell us the maximum number of vertices that your mesh can have. Explain why (include the actual math).
  - Tell us the maximum number of triangles that your mesh can have. Explain why (include the actual math).
  - Explain why it is important to make the mesh file format human-readable. (As part of this you should mention changes we will make in future assignments and explain why for this assignment we prioritize making the mesh human-readable over other considerations.)
  - Show us what your mesh file format looks like
    - Explain each part, and tell us why you believe that the format you chose is the most human-readable choice
      - (In particular, if there are places where there were several choices that you could have made I am interested in hearing why you made your specific choice)
    - Tell us whether the winding order is right or left handed, and explain why you made that choice
    - Tell us whether your color requires 3 or 4 values (or whether both are acceptable), and explain your reasoning behind that choice
    - Tell us what file extension you chose and why
  - Did you put your authored mesh in a subdirectory of $(AuthoredAssetDir)? Why or why not? Did you build your mesh to a subdirectory of $(BuiltAssetDir)? Why or why not?

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- If you change the position of a vertex in your file and then build the solution and run the game you will see the difference
  - For example, changing (0,0) to (-0.5,-0.5)
- If you change the color of a vertex in your file and then build the solution and run the game you will see the difference
- If you put an error in the mesh file (for example, I create an empty line at the top of the file and type asdf) and build the solution and run then:
  - The user will see some message that something has gone wrong and the game will exit without crashing
  - The error that Lua reported will be in the generated log file, with the name of the mesh file with the error so that a programmer can figure out what went wrong after the fact
  - In a debug build an assert will fire (i.e. a message will show that an assertion failed) so that a programmer can break into the debugger to try and figure out what happened

## Details

### Indexed Drawing

- You will have to add an "index buffer" to your mesh
  - In Direct3D this will be an ID3D11Buffer*
  - In OpenGL this will be a GLuint, and like the vertex buffer it will be encapsulated by the vertex array object. (In other words, you should keep it around when EAE6320_GRAPHICS_ISDEVICEDEBUGINFOENABLED is #defined, but otherwise release it during initialization after it has been used to create the vertex array object.)
- You will have to add code to clean up the index buffers
  - You should be able to figure out how to do this based on the clean up code for vertex buffers
- You will have to add a uint16_t to store how many vertices a mesh has and a uint16_t to store how many indices it has
- You will have to change the way you create a vertex buffer to include the minimum number of vertices needed to draw a square
  - The data and order of vertices should be the same for each platform
- You will have to create an index buffer and populate its initial data in the same place where you are creating and populating the vertex buffer
  - The indices will have a different order in each platform, because Direct3D requires triangles with a left-handed winding order and OpenGL requires triangles with a right-handed winding order
  - In Direct3D, creating and initializing an index buffer is very similar to creating and initializing a vertex buffer, except:
    - You should be able to figure out how much space is needed
    - The "bind flags" will be D3D11_BIND_INDEX_BUFFER instead of D3D11_BIND_VERTEX_BUFFER
  - In OpenGL, creating and initializing an index buffer is very similar to creating and initializing a vertex buffer, except:
    - You will bind the buffer with GL_ELEMENT_ARRAY_BUFFER instead of GL_ARRAY_BUFFER
    - When you allocate and assign the initial data you will also use GL_ELEMENT_ARRAY_BUFFER. You should be able to figure out how much space is needed.
- You will have to change the draw/render code to use indices
  - In Direct3D:
    - You will still have to set the vertex buffer and the primitive topology as you have in previous assignments. In addition, though, you will also have to set the index buffer and then make a different draw call. Your code should look something like this:

       ```cpp
       // Bind a specific vertex buffer to the device as a data source
       {
         EAE6320_ASSERT( [INDEXBUFFER] != NULL );
         // Every index is a 16 bit unsigned integer
         const DXGI_FORMAT format = DXGI_FORMAT_R16_UINT;
         // The indices start at the beginning of the buffer
         const unsigned int offset = 0;
         context.m_direct3dImmediateContext->IASetIndexBuffer( [INDEXBUFFER], format, offset );
       }
       // Render triangles from the currently-bound vertex and index buffers
       {
         // It's possible to start rendering primitives in the middle of the stream
         const unsigned int indexOfFirstIndexToUse = 0;
         const unsigned int offsetToAddToEachIndex = 0;
         [IMMEDIATECONTEXT]->DrawIndexed( [INDEXCOUNT], indexOfFirstIndexToUse, offsetToAddToEachIndex );
       }
       In OpenGL:
       You will still have to bind the vertex array object as you have in previous assignments, but you will have to make a different draw call. Your code should look something like this
       // Render triangles from the currently-bound buffers
       {
         // The mode defines how to interpret multiple vertices as a single "primitive";
         // we define a triangle list
         // (meaning that every primitive is a triangle and will be defined by three vertices)
         const GLenum mode = GL_TRIANGLES;
         // Every index is a 16 bit unsigned integer
         const GLenum indexType = GL_UNSIGNED_SHORT;
         // It's possible to start rendering primitives in the middle of the stream
         const GLvoid* const offset = 0;
         glDrawElements( mode, [INDEXCOUNT], indexType, offset );
         EAE6320_ASSERT( glGetError() == GL_NO_ERROR );
       }
       ```

### Mesh File Format
- It may help to look at readNestedTableValues.lua in the Using Lua example
- The primary goal of this file is to be human-readable
  - Someone with domain knowledge should be able to open one of your mesh files and understand it
    - John-Paul, Saurabh, any of your fellow class mates, or any professional graphics programmer should intuitively be able to understand your mesh file format
  - You don't need to worry about someone who doesn't know anything about graphics! For example:
    - It's ok to assume that if I see a position represented as two numbers I will know they mean XY
    - It's ok to assume that if I see a color represented as three numbers I will know they mean RGB, and that if I see a color represented as four numbers I will know they mean RGBA
    - It's not ok, however, to assume that if I see a set of two numbers followed by a set of three numbers that I will know the first represents POSITION and the second represents COLOR!
- You should prioritize making your format easy to read and understand over making it quick/easy to author
  - These mesh files will eventually be created by Maya (in other words, you will create an exporter for Maya that saves out meshes in this format you create), and so you don't need to worry about how hard it is for a human to create. As a human, however, you will need to occasionally read these files to debug problems, and so the easier to read and the more understandable you make your format the easier it will be for you to debug issues.
- You should prioritize making your format easy to read and understand over making it efficient to load
  - These mesh files will eventually be converted by a tool you create into a different binary format that can be read as efficiently as possible by your engine at run-time. In other words, the format you create for this assignment will be a human-readable intermediate format, and it will eventually only be loaded at build-time.
- Some of the data in your format should be named (i.e. stored in a table acting as a dictionary) and some should be ordered (i.e. stored in a table acting as an array). When designing your format you should ask yourself about each piece of data:
  - Does the order of this data matter?
    - If the order does matter then you should use an array
    - If the order doesn't matter then you should ask yourself:
      - Does this data need a name or label to be understandable?
        - If it does need a name to be understandable then you should use a dictionary
        - If it doesn't need a name to be understandable then you can use either an array or a dictionary, but usually an array will be the better choice (because if a name doesn't matter then providing one can be misleading and confusing)
- You will have to include a nested table for vertex data and a nested table for index data
  - The vertex data table will have to include a nested table for each vertex in the mesh
    - Each vertex will have to include a nested table for position and a nested table for color
      - (My recommendation would be to get it working with just position and then add color later)
    - The position should have two values (for x and y), but remember that in a few assignments we will change this to have three values (for x, y, and z)
      - (Do not add three values now! Instead design your format and code so that it is easy to expand when we need to.)
    - The color values must be stored as [0,1]
      - (In other words, they must be stored as floating point values and not as [0,255] byte values like we use at run-time. You will have to convert these values when you load them.)
    - The run-time requires 4 color values (RGBA), but in our class the A value will always be 255. You can decide whether your format requires all 4 values, whether it requires only 3 RGB values (in which case your mesh loading code will fill in the implicit 255), or whether it will accept either 3 or 4 values, in which case your mesh loading code will check to see if a 4th value exists and use it if it does but fill in the implicit 255 otherwise.
      - (Remember that eventually Maya will create mesh files in this format for you; it is easy to have your Maya exporter add an alpha value of 1 if your format requires it, so make this decision based on what you think is the most readable and understandable rather than on what is the easiest to create by hand)
  - The index data table will have to include 3 indices for each triangle, but there are different strategies for how to represent this. You should think about what makes the most sense to you and design your format accordingly.
- Remember to prioritize making your format as easy to read and understand for a human as possible! Some ways of representing indices will require extra code to load the Lua file, but you should not let that influence your decision. Instead, you should design the format to be as easy to understand as possible, and then write the loading code to match. (When we create the binary file format we will do the opposite and design it to load as quickly as possible.)
- You will have to choose an extension. I named my file "square.mesh" in my reference implementation, but you can choose anything that makes sense to you. Keep in mind that we will eventually have two different mesh formats (a human readable source and a binary target), so you will have to decide whether to use the same extension for both or to make a distinction.

## Creating a Square Mesh File

- Converting the hard-coded mesh data to your file format should be mostly trivial if you designed your format well. There are really only two things that might be tricky:
  - The authored mesh file can only have a single winding order, so you will have to choose whether to make it left or right handed (and then convert one of the platforms at run-time when you load the file).
    - My advice is to make the winding order right-handed because that is what Maya uses (which will simplify your exporter). It really doesn't matter, though, and you can choose what makes the most sense to you.
  - If you do this requirement after you have added color to your vertices (not what I recommend) then remember to convert the colors from [0,255] to [0,1]
- You will have to update your solution so that your mesh file is available to be loaded by your game. You should know how to do this.

## Loading the Mesh File

- You must understand C code in the Tables project in the Using Lua example in order to do this. If you haven't already then please step through the code in each CPP example to make sure that you understand how it works before attempting to load the file in your own solution.
- You will have to restructure your mesh initialization code:
  - Your mesh's public interface should now have a Load function that accepts a file name as input
    - This is what your game code will call. A user should be able to say "I want an instance of a mesh using the square that an artist made for me"
  - Your existing initialization function (with a platform-independent interface that creates a platform-specific vertex and index buffer) should now be made part of your mesh's private implementation. It should now take an array of vertices and an array of indices as input and use them to create the buffers rather than the hard-coded data.
    - (It will also need to know how many vertices and how many indices the arrays hold. Depending on how you represent the mesh you may also have to pass those in)
  - The Load function will be almost entirely platform-independent, and once it loads the the vertex and index data it will then provide that data to the private initialization function
    - The only part of loading the mesh data from a file that can be platform-specific is that you will have to adjust the winding order of triangles. You will have to switch some index values for one of the platforms (which one depends on which winding order you store in your file).
    - You may wonder why you should switch the order in the platform-independent file-loading function instead of the platform-specific vertex/index buffer creation function. Good question! The reason is because in a future assignment we will move the Lua code into a builder tool that will create a binary file, and that is where we want any logic to live. When we make this future change we will then load the binary file at run-time, and we want to do that as efficiently as possible which means doing as little work as possible.
- Some tips:
  - You will have to add Lua to your game. If you understand the Using Lua example you should know how to do this.
  - You should create a local Lua state every time you load a mesh, load the data, and then close the Lua state
    - Normally this would be bad practice, but it will make more sense when we move the Lua loading code out of the game and into a MeshBuilder tool.
  - Since we are loading data from a file we no longer know how many vertices or indices a mesh will have ahead of time. This means:
    - You will have to extract the vertex count and the index count from the Lua file
    - You will have to dynamically allocate the space for the vertices and for the indices, and fill in that allocated memory with the data from the Lua file
    - You will pass the dynamically-allocated vertex and index arrays into your initialization function, which will create the platform-specific vertex and index buffers. Once initialization is complete you must free the dynamically-allocated memory (regardless of whether the vertex or index buffers were successfully created). This should happen in the Load function, and the only data you hold on to will be the vertex and index buffers (just like in previous assignments) and the number of vertices and indices.
    - You should use malloc/free rather than new/delete
      - This is because we don't want any constructors or destructors to be called
      - When you want to actually create a run-time class or struct (like when your game creates a new mesh, for example), then you should call new/delete
      - When, on the other hand, you want to allocate a block of binary data (like when you are loading binary data from a file, for example), then John-Paul's opinion is that malloc/free is a better choice.
  - If there is an error while loading the mesh you should output an error to your log file. When we create a MeshBuilder tool we will change this so that we output an error that will show up at build-time in Visual Studio's Error List, but we should be able to use the same error messages.
   - Consider checking for every error that you can think of. Writing error-checking code is annoying, but good error checking can make the difference between catching a bug immediately and spending hours trying to track down confusing behavior.
    - (For example, if you verify that you never have more vertices or indices than you can store then you will immediately get an error if an artist (or you, in a future assignment) create a mesh that won't work. If you don't verify this, though, then you may see behavior that doesn't make any sense to you; the bug may be easy to fix, but it will take a lot of wasted time and energy to discover what is going wrong.)

### Adding a Color to Each Vertex

- You will need to add a color to each vertex in your mesh file
  - How to do this is discussed in the Mesh File Format section above
- You will need to add four uint8_ts to the sVertex struct
  - These store red, green, blue, and alpha in that order
  - If you haven't already put this struct definition in a platform-independent place you may consider doing so in this assignment so that you don't have to make changes in two different places
- You will need to add code to your Lua-loading code to read in each color channel and store it in the sVertex. The code to do this should be very similar to how you load position for each vertex, except:
  - You will have to load 3 (or 4) RGB (or RGBA) values instead of 2 XY values 
  - You will need to convert each [0,1] floating point value to a [0,255] integer value
    - Conceptually this is easy: uint8_t o_value = static_cast<uint8_t>( i_value * 255.0f )
    - The above code has problems, though: What happens if the mesh file has an input value less than 0 or greater than 1? Additionally, casting a float to an int truncates the decimal values, but in this case we would like to round up if the decimal value is 0.5 or greater. Can you write a better conversion function?
- You will need to add code in the vertex and fragment shaders to use the color
  - I have provided shaders that you should use hereDownload here
  - You should add the vertex position animation from your previous assignment to the vertex shaders. (Or you can add different animation if you wish.)
  - For this assignment leave the fragment shaders as they are so that we can see the vertex color and nothing else.
- You will need to add code to pass the vertex colors from your mesh to the vertex shaders
  - In Direct3D:
    - You will need to change the code where the vertex input layout is created. Currently it specifies the XY position, and you will need to add the RGBA color.
    - How big should the array of D3D11_INPUT_ELEMENT_DESC be if you want to add a new one?
    - You will need to fill in the new Color element just like the Position element is done except:
      - The SemanticName should be "COLOR" (see vertexShader.hlsl to see where this comes from)
      - (The SemanticIndex should be 0, since the shader doesn't specify an index)
      - The Format should be DXGI_FORMAT_R8G8B8A8_UNORM
      - You will have to set the AlignedByteOffset to be the offset of the red color channel from the first of the sVertex struct. Can you figure out how to do that?
  - In OpenGL:
    - You will need to change the code where the vertex format is set up (search for the calls to glVertexAttribPointer() and glEnableVertexAttribArray()). Currently it specifies the XY position, and you will need to add the RGBA color.
    - You will have to add another call to each of the functions listed above
    - The data given to those calls for the new Color element will be just like the Position element except:
      - The vertex element location should be 1 (see vertexShader.glsl to see where this comes from)
      - What should elementCount be? (How many uint8_ts does Color have?)
      - isNormalized should be GL_TRUE
      - The type should be GL_UNSIGNED_BYTE
- An example screenshot from my reference implementation (without vertex animation) is:
- assignment04_screenShot.png
- You do not have to choose the same colors! Be creative!
- 