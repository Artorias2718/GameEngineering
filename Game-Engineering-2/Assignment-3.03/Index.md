---
---

# Assignment 3.03 - Mesh Interface

## Requirements

- Create a representation of a "mesh" in your Graphics project
  - There must be a single separate header file that uses #ifdef/#endif for the platform-specific data (see Small Differences)
  - There must be two platform-specific CPP files, and they must be set up on disk and in Solution Explorer as described in Big Differences
    - (You may also have a platform-independent CPP file for shared code)
  - Your mesh should encapsulate the following data that has been in the Graphics.[platform].cpp files:
    - Direct3D: ID3D11Buffer* (for the vertex buffer)
    - OpenGL: GLuint (for the vertex array ID) and GLuint (for the vertex buffer ID when something is #defined)
  - Your representation must have the following functionality:
    - Initialization
    - Clean Up
    - Draw / Render
  - The Draw/Render function's interface must be platform-independent
- Create a way for your game to do things per-frame
  - Create a virtual function in cbApplication, and call it in the proper place in OnNewFrame()
  - Override this virtual function in MyGame
- Create a mesh in your game and submit it to be drawn/rendered
  - You will have to #include the mesh header file in MyGame and set up the appropriate libraries and build order dependencies
  - You will have to initialize the mesh and clean it up in MyGame
  - Every frame you will have to call a Graphics function to say "draw [this mesh] this frame"
    - This code must be platform-indepndent! You should not add any platform-specific code to MyGame.
- Create a way to submit things to be drawn/rendered each frame in Graphics 
  - The interface for this must be platform-independent, and only reference your mesh representation
  - Your Graphics system RenderFrame() function should be changed in each platform to render the meshes that have been submitted that frame
    - The code that you add to draw/render a mesh in the RenderFrame() function must be platform-independent! Even though you will have to add it to both platforms the code you add should be identical because you have made a platform-independent interface for your mesh.
- Your write-up should:
  - Show a screenshot of your game running
    - (Nothing will have changed visually since Assignment 02, but it's always more interesting for your readers to have at least one picture)
  - Show us your mesh header file
    - Describe why you designed your mesh the way that you did. You may want to say why you chose a class or a struct. You should definitely tell us how you were able to make the draw/render function platform-specific, and what challenges you faced.
  - Show us your code in MyGame that submits the mesh to be drawn
  - Show us your code in Graphics.[platform].cpp that calls the function to draw a mesh. (Not the implementation in the mesh CPP file, but the actual use of your platform-independent mesh draw/render interface so that we can see your platform-independent design in action.)

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up

## Details

### Mesh

- You may either use a C++ class or a plain struct
- A mesh can be thought of as geometric data (which provides the form of an object). This is distinct from the "material" that an object is made out of.
  - For example, imagine a scene with a block of wood and a block of metal. In the terminology of our class, we could represent both objects with a single mesh (the block), but each object would use a different material (wood and metal). It is possible to render a single mesh with different materials.
  - As another example, imagine a scene with a wooden block on top of a wooden table. Two different meshes would be required for the scene (a block and a table), but they could both use the same material (wood).
- Don't move any data from Graphics.[platform]cpp besides the data mentioned in the requirements (the ID3D11Buffer* for D3D and the vertex array and buffer GLuints for OpenGL)
  - It is ok to add other data to your mesh if you think it's a good idea (for example, you might want to store the number of triangles). Any extra data you decide to add should be platform-independent, though. The only platform-specific per-mesh data (i.e. data that contributes to the size of an individual mesh) should be the things mentioned in the requirements.
- Don't make this requirement more complicated than it has to be! Students often struggle with this more than they have to, so please consider the following advice:
  - The most important thing to consider for this assignment is the interface. You should ask yourself "how can I take these chunks of code from these two different Graphics.[platform].cpp files and make a nice common interface for them?"
  - Once you have an interface designed (in a header file), then filling in the implementation should be relatively simple: You should just move code from the Graphics.[platform].cpp file into the corresponding function in the platform-specific mesh CPP file.
    - You may have to change variable names, but other than that you the actual implementation code should be almost identical.
    - A good way to go about this is to do a search for all of the code in Graphics.[platform].cpp that uses the variables, and then to figure out how to move each chunk of this to your mesh representation. For example, in D3D I would search for "s_vertexBuffer", and know that anywhere I saw that variable used I would have to move it to my mesh representation.
    - I haven't taught you enough about the graphics APIs to expect you to do anything fancy. If you find yourself doing much more besides cutting/pasting API code from one place to another and changing a few variable names (or ways to access API objects), ask yourself "am I making this more complicated than it has to be?"
- If you follow the advice above, then there are really only two parts of this requirement that might be tricky:
  - How to deal with the sVertex struct?
    - This struct is used by different pieces of code. Can you figure out a way to share it?
  - How to deal with D3D "device" objects?
    - In D3D you need a device to create the vertex buffer and device immediate context to draw / render, but these objects aren't part of the mesh (your game will have many meshes, but only one device and only one device immediate context). Can you figure out a way to let the Initialize() and Draw/Render() functions have access to these objects while still keeping their interfaces platform-independent?
      - (There is no "right" answer to this. There are several strategies, and all of them have different advantages and disadvantages. For some of you this might be obvious and easy, and for some of you it might be really difficult. If you struggle with it that's ok! Do the best that you can, and we will discuss different solutions in class after everyone has had a chance to try and come up with their own.)

## Submitting Things to be Drawn

- Eventually your game will have many "things" (or "objects" or "entities"). A "thing" might have lots of different "things" associated with it, like AI, physics, sound, etc. When it comes time to actually draw a "thing" to the screen, however, we want to have an interface where we can just pass the data that the Graphics system needs.
  - Right now the only data that we have is a mesh, which you have created for this assignment. In the future, though, there will also be a position, orientation, and material.
- Your game logic needs to be concerned about the state of objects, but the Graphics system doesn't: It just wants to know what to draw every frame. Every frame it expects the game to tell it what to draw, and then it draws those objects.
  - This means that the Graphics system needs some kind of list of renderable data, and every frame it waits for the game to submit renderable data. The game submits all of its renderable data, and then tells the Graphics system "ok, render it all!". The Graphics system then iterates through its list of renderable data, renders it, and then clears the list so that it's ready for the next frame.