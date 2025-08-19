# Line Drawing

## Debug Line Drawing - Author: John-Paul Ownby

- You will need an API for the user to draw a debug line by specifying the following:
  - Starting point (in world space)
  - Ending point (in world space)
  - Color
- An example implementation looks like this:
  - ```void AddLine( const D3DVECTOR& i_start, const D3DVECTOR& i_end, const D3DCOLOR i_color = D3DCOLOR_XRGB( 255, 255, 255 ) );```
- Doing it this way means that the line will only draw for the frame when the call is made! You may choose to make things more complicated by allowing the user to specify that the line persists (maybe by giving how many seconds it should display or even by making an object that is explicitly created and destroyed).
- You should be able to draw all of the lines in a single draw call by using a "line list". This is an example:

  ```cpp
  const D3DPRIMITIVETYPE lineList = D3DPT_LINELIST;
  const unsigned int indexOfFirstVertexToRender = 0;
  result = i_direct3dDevice.DrawPrimitive( lineList, indexOfFirstVertexToRender, s_debugLines.size() );
  ```

- In order for this to work you will need to create a vertex buffer that is writable, and you will need to specify the max number of debug lines that could be drawn in a single frame (remembering that there are two vertices per-line). Then, every time that a user adds a line to be drawn you will keep a record of the points and the color. When it comes time to draw the lines, if there are any for that frame to draw you:
  - Lock the vertex buffer
  - Copy the data you have been collecting
  - Unlock the vertex buffer
  - Clear the data
  - Draw the lines (using standard techniques, including setting the shaders, constants, stream source, and making the draw call)
- You will need new shaders
  - The vertex shader should pass in the vertex in world space (because that is how the user will define the lines). You will still transform the position from world to view, and then from view to screen space; you will just skip model space
  - The vertex shader should take a color as an input (specified by the user) and pass it on unchanged to the fragment shader. The fragment shader should do nothing except output that color
- The lines should draw in 3D space. This means they can either draw before or after your 3D geometry, but they should draw before your 2D sprites.
- These should only draw in debug builds (as always I encourage making a specialized #define), and you should create a new PIX event for them