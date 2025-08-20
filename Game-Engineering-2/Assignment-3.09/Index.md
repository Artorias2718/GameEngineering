---
---

# Assignment 3.09: Binary Mesh Files

## Requirements

- Change your MeshBuilder to output a binary mesh
  - You will have to move the code that reads the human-readable mesh file from run-time to MeshBuilder
  - There are two functions you may use to output binary data:
    - The C way is fwrite()Links to an external site.. The link shows an example of opening a file, writing to it, and closing it.
    - The C++ way is std::ofstream::write()Links to an external site.. The link shows an example of opening a file, writing to it and closing it.
- There are four pieces of binary data that you must output:
  - The number of vertices
  - An array of vertex data
  - The number of indices
  - An array of index data
- There should be exactly four calls to fwrite()/std::ofstream::write() to write out each piece of binary data
  - You should not have to iterate through each vertex or each index to write them to your binary file! You should be able to write out the entire array as a single block with a single function call.
  - (It is possible that you may have more than four calls to fwrite()/std::ofstream::write() if you add any of the advanced optional features discussed in the details section, but even in that case you should still only have four calls for each of the four pieces of required data.)
- Change your run-time mesh representation to load a binary mesh
  - Your platform-independent mesh code will have to extract the four pieces of data that you wrote to your binary file
    - (Your platform-specific mesh code should not have to change)
  - You should use Platform::LoadBinaryFile() to load the entire file as a chunk of memory
  - There should be exactly four calls to extract each piece of binary data
    - You must not iterate through vertices or indices!
      - Every year I say this and every year some students do it anyway. <span style="color:#f00;">NO ITERATION!</span> You should not have any for or while loops anywhere in your mesh loading/initialization code!
  - Once the mesh is initialized you should free the Platform::sDataFromFile
- Add a mesh that is a recognizable object (that isn't just a Maya polygon primitive)
  - The easiest way to do this is to find something online, but if you have objects that you've made or that you have permission to use from your project you can use that too
    - HereLinks to an external site. is one possible site you could use
  - Most meshes will rely on textures to look correct, which our engine doesn't support yet. You may want to add some vertex color to make it easier to see what the new mesh is.
  - You may use an existing object for this requirement if you have already done this in a previous assignment
- Create a run-time representation of an effect
  - (Only the run-time code should change for this assignment. We will create an effect file later.)
  - Your effect should encapsulate the following data:
  - Direct3D:
    - ```ID3D11VertexShader*, ID3D11PixelShader*, ID3D11InputLayout*```
  - OpenGL:
    - ```GLuint (program ID)```
  - Your effect should have the following interface:
    - Load
    - Clean Up
    - Set/Bind
  - For this assignment the parameters that are passed to the load function should be the path to the vertex shader and the path to the fragment shader
  - You should update Graphics.[platform].cpp so that it no longer has shaders, but instead has an effect that it loads, sets/binds (before making the mesh draw calls), and cleans up.
- Your write-up should:
  - Show us a screenshot of your awesome mesh!
  - Show an example of a binary mesh file built by your MeshBuilder (a screenshot of the file in a hex editor is fine) and:
  - Tell us the order of the four things in the binary file and why you chose that order
    - Help us to be able to recognize the data. If you can highlight the four things in different colors, for example, that can really help your readers to understand your binary file format.
  - Tell us at least two advantages of using binary file formats (I am looking for two specific reasons, but you can include more if you can think of them)
    - Compare the advantages of binary formats with the advantages of human-readable formats. Why do we use binary formats at run-time in our class but human-readable formats to store the data?
  - Tell us whether the built binary mesh files should be the same or different for the different platforms, and explain why. (In other words, should the binary file for the square mesh that your MeshBuilder outputs for Direct3D be the same as the one that it outputs for OpenGL? If so, explain why. If not, explain why there are differences and what they are.)
  - Show us how you extract the four pieces of data from binary data at run-time
    - Unless you chose to do the optional enhancements listed in the details section your code to do this should be fairly simple. If it isn't you are probably doing something wrong.
  - Create a mesh in Maya with many vertices and indices to measure the advantages of binary files
    - A helix is a good choice for this because there are many parameters that can greatly increase the vertex/index count. Make it have as many as you can that will still fit within the limits of uint16_ts and 16-bit indices. You don't need to commit this to git; just make it temporarily to do measurements for your write-up and then delete it.
    - Tell us how big (in bytes) the human-readable version is on disk and how big (in bytes) the binary version is on disk
    - Tell us how long it takes to load the human-readable version and how long it takes to load the binary version
      - You can use Time::GetElapsedSecondCount_total() for this. Make this temporary code and revert it after you have made measurements!
      - To time the human-readable version put code in MeshBuilder. Make sure to time not just how long it takes to load the Lua file but how long it takes to extract the data. (You should start timing right before loading and end timing right before writing out the binary file.) You can put a temporary print statement with the total time that will show up in Visual Studio's Output window.
      - To time the binary version put code in your run-time mesh loading code. Make sure to time not just how long it takes to load the binary file but how long it takes to extract the data. You can put in a temporary logging statement with the total time that will show up in your log file.
      - Make sure to do this timing on a release build! (It doesn't matter which platform you time, though.)
  - Show us your code that sets/binds your effect in a platform-independent way (i.e. show us the code in Graphics.[platform].cpp that does this, not the actual implementation)

## Submission Checklist

- Your write-up should follow the standard guidelines for submitting assignments and the standard guidelines for every write-up
- If you add a bug to a mesh (like typing "sdf") and then build assets 1) the build should fail, 2) an error message should be displayed in Visual Studio's Error List window, and 3) double-clicking that error should open the file

## Details

### Loading the Human-Readable Mesh File in MeshBuilder

- Most of the code to read the Lua mesh file that you used at run-time should be the same when you move it to MeshBuilder, but one big required change is how errors are handled:
  - Instead of logging errors you will have to call ```eae6320::AssetBuild::OutputErrorMessage()``` so that errors will show up in Visual Studio's Error List window
  - You should remove asserts. You may decide to leave a few as sanity checks, but an assert firing at build-time when a debugger is seldom useful.
- When organizing your MeshBuilder code keep in mind the four pieces of data that you will need to write out as binary data
  - In previous assignments you had to extract these same four pieces of data from the Lua file at run-time so you could create the Direct3D and OpenGL vertex buffer and index buffer. In this assignment you will be extracting them so that you can create a binary file.
  - This part of your MeshBuilder that deals with Lua, then, should be self-contained. You will input the path to the human-readable Lua file, and the output should be the four pieces of data. Once you have those four pieces of data you can output them as a binary file without worrying about where they came from. (This would allow you, for example, to change to a different format in the future if you decide you don't like Lua after this class is over.)

### Writing Binary Data

- There are several possible correct orders that you could choose to write out the four pieces of data, but also some that are incorrect
  - When deciding how to write data to a file keep in mind what you will have to do at run-time to read it in. Some of the data is independent (meaning that it could be read in any order), but some of the data must be read first before other data can be read. As long as you get these dependencies correct you can choose any order that you wish (although some orders may be more efficient than others).
- When dealing with binary data the size of numbers is critical. Programmers are sometimes careless with the size of numbers and allow the compiler to implicitly convert between them, but this can't be done with binary data. Always use explicit sizes (e.g. uint8_t, uint16_t, uint32_t, or uint64_t instead of unsigned int), and if you need to read a uint16_t make sure that you have written a uint16_t.
- Once your MeshBuilder is generating binary files you should check the results in a hex editor to make sure that the file looks like what you expect
  - I like the way that Visual Studio shows binary data, but recent versions make it really annoying to actually get a binary file open (it tries to be smart about what the file is rather than just showing it as binary data) and so I rarely use it anymore. My current preference is HxD (Links to an external site.).
  - You should know the order of the four things and how many bytes they each take up. Verify that the file has the right data in the right places. (It might be hard to recognize the float data, but you should definitely be able to find the number of vertices and indices/triangles, and the array of indices itself should also be fairly readable if you know what you're looking for.)

### Extracting Binary Data

- You will have to locate the address of each of the four things starting from the base address of the chunk of binary data that was read from the file. The offsets of each of the four things from the base address will depend on 1) the order that you wrote the data out and 2) how many vertices and indices there are in the arrays.
- The first thing you wrote out will be located at the base address, so this is easy. As an example let's assume that the first thing in your binary mesh file is the number of vertices as a uint16_t. You could then extract that information like this:
  - ```uint16_t vertexCount = *reinterpret_cast<uint16_t*>( data );```
- If the vertex count is a uint16_t then you know that the next piece of information (whatever it is) will be located at 2 bytes after the base address. (You must understand the previous sentence in order to complete this assignment. If it's confusing to you then re-read it as many times as necessary until you understand what it's saying and why.)
  - If the next piece of data is the number of indices stored as a uint16_t then you could write:
    - ```uint16_t indexCount = *reinterpret_cast<uint16_t*>( data + 2 );```
- If, on the other hand, the next piece of data was the array of vertices then you could write:
  - ```sVertex* vertices = reinterpret_cast<sVertex*>( data + 2 );```
- Note that the syntax I'm using above will only work if data is a char* or uint8_t*. If it's a void* like Platform::sDataFromFile::data the compiler won't let you add an offset, and you'll have to cast it in order to use pointer arithmetic like this assignment requires. Instead of what I show above my recommendation would be to create an entirely new uint8_t* that points to your current place in the data, in which case you would add an offset to that temporary variable every time you extract a new piece of information.
- Adding an explicit 2 as a hard-coded "magic number" as I show above is generally bad practice. Can you think of what you should use instead?
- The vertex count and index count will have fixed sizes that will be the same for every mesh, but the size of the actual vertex and index arrays will be different. You can calculate how many bytes an array takes up in your block of data by taking into account how many bytes a single element takes up as well as how many elements there are in the array.

### Optional Binary Mesh Enhancements

- The following are things you can try if you are interested. They are fun and interesting, but my recommendation would be to wait until all of the assignment is done before trying them, since they can make things significantly more complicated.
- Alignment
  - You can write out binary data any way that you want, but certain data will work better with certain alignments when you read it in. In this current assignment it doesn't really matter because we free the data after we have initialized the mesh (because the API keeps its own copy), but you could still align things optimally. In order to do this you 1) need to be a bit more careful about the order of data, taking size and alignment into account, and 2) may need to add "padding" to your file.
  - If you add padding when writing you'll need to figure out how to deal with it when reading the file in. It's possible to make it implicit (you calculate it at run-time the exact same way that you did at build-time), or you can write it out explicitly in the file. If you write it in the file think about the smallest size you can make it (how big could the padding possibly be?) and take that into account when figuring out the ideal alignment for everything else.
  - You should use alignof() to get the correct alignment of sVertex even if it changes in future assignments.
  - The actual contents of any padding doesn't matter, but you should always try to make your generated files deterministic (in other words, the same file will always be generated). A good choice is to use zero for any padding.
- 32-bit indices
  - We have used 16-bit indices to keep our mesh sizes as small as possible. This is fine for most meshes, but you may encounter cases where a mesh has more vertices than can be indexed with only 16 bits and these cases would be unusable in our class with our current code.
  - If you want to support 32-bit indices then you need to update your code to be able to use either 16-bit or 32-bit indices. At build-time you would need to calculate whether 16 bits are sufficient and then generate an array of either uint16_ts or uint32_ts and write out something in the binary mesh which makes it clear what kind of indices are being used. At run-time you would need to read this in and adjust the code for both Direct3D and OpenGL accordingly.
  - If you decide to support this you should use uint32_ts for the number of vertices and number of indices
  - This is a fun change to make but it's more challenging than it may seem. If you try this please make sure that you first commit your working changes that fulfill the assignment's requirements. Then, if something goes wrong you can just give up and revert everything to get back to a working finished assignment.