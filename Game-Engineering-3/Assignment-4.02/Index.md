---
---

# Assignment 4.02: Debug Shapes

## Requirements

- Add functionality to draw debug shapes
  - Line
  - Box (wireframe)
  - Sphere (wireframe)
  - *optional: +1 extra credit* Cylinder (wireframe)
  - Place at least **two examples** of each of these shapes near the center of the level. Each shape instance must be a different color.
- The functionality to draw debug shapes must completely vanish from your code in a release build.
  - Wrap your functions in ```#ifdef _DEBUG```
- [Line drawing in DirectX (from John-Paul)](./LineDrawing.md)
- Draw a sphere using API calls
- DirectX: https://msdn.microsoft.com/en-us/library/windows/desktop/bb172795%28v=vs.85%29.aspxLinks to an external site.
- OpenGL: https://www.opengl.org/sdk/docs/man2/xhtml/gluSphere.xmlLinks to an external site.
- Draw a sphere manually: http://richardssoftware.net/Home/Post/7Links to an external site. (also includes box and cylinder)
- Alternatively, an Icosphere (geodesic sphere): http://blog.andreaskahler.com/2009/06/creating-icosphere-mesh-in-code.htmlLinks to an external site.