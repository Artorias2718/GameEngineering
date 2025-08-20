---
---

# Assignment 4.06: Octree - Part 2

## Requirements

- Execute all ray-cast operations through the Octree.
- Create run-time Octree code that can test against the ray.
- If you chose to handle spanning triangles by duplication, make you you don't test against the same triangle twice in different octants.
- Visualize a raycast through the Octree
- Add a Debug menu check box to enable casting a ray through the position and facing direction of the player character. This should be a ray, not a segment... no endpoint until it hits collision geometry.
- Using debug shapes, draw the ray that was cast, along with all Octree nodes at all depths that the ray intersected. Use colors to represent depth.
- *optional* - highlight the triangle(s) that the ray hit via wireframe render or color tinting.
- The ray cast should not be attached to the fly camera in any way, allowing the fly camera to be used to inspect the results.
- You'll need a function to test if a ray collides with an AABB... like this one:
  ```cpp
  int TestSegmentAABB(Point p0, Point p1, AABB b)
  {
    Point c = (b.min + b.max) * 0.5f; // Box center-point
    Vector e = b.max - c; // Box halflength extents
    Point m = (p0 + p1) * 0.5f; // Segment midpoint
    Vector d = p1 - m; // Segment halflength vector
    m = m - c; // Segment midpoint relative to box center
     
    // Try world coordinate axes as separating axes
    float adx = Abs(d.x);
    if (Abs(m.x) > e.x + adx) return 0;
    float ady = Abs(d.y);
    if (Abs(m.y) > e.y + ady) return 0;
    float adz = Abs(d.z);
    if (Abs(m.z) > e.z + adz) return 0;
     
    // Add in an epsilon term to counteract arithmetic errors when segment is
    // (near) parallel to a coordinate axis
    adx += EPSILON;
    ady += EPSILON;
    adz += EPSILON;
     
    // Try cross products of segment direction vector with coordinate axes
    if (Abs(m.y * d.z - m.z * d.y) > e.y * adz + e.z * ady) return 0;
    if (Abs(m.z * d.x - m.x * d.z) > e.x * adz + e.z * adx) return 0;
    if (Abs(m.x * d.y - m.y * d.x) > e.x * ady + e.y * adx) return 0;
     
    // No separating axis found; segment must be overlapping AABB
    return 1;
  }
  ```