# Assignment 4.05: Octree - Part 1

## Requirements

- Organize all the static collision data into an Octree of at least depth 3.
  - Write out the Octree during export via your Maya plugin code.
    - Keep the old collision data ("triangle soup") for now so your player still collides properly. 
- You must handle triangles that span across quadrants in one of three ways:
  - Put the triangle into every quadrant it touches. (easy)
  - Split the triangle (difficult)
  - Make a "Loose" Octree instead (moderate)
  - Though it does solve the problem, bubbling up the spanning triangle to parent quadrants is not allowed for efficiency reasons.
- Visualize the Octree
  - Add a Debug Menu checkbox to draw the full octree using debug boxes.  Use different colors to represent depth (red->green->blue->yellow->etc)
- More info on Octrees:
- [Octree - Wikipedia](http://en.wikipedia.org/wiki/Octree)
- [QuadTrees and Octrees](http://www.cs.berkeley.edu/~demmel/cs267/lecture26/lecture26.html#link_3)
- [LooseOctreePaper.pdf](https://anteru.net/blog/2008/loose-octrees/).
- Game Programming Gems: An easy version of a Loose Octee is found in this book