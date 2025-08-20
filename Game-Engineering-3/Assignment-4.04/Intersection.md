---
---

```cpp
// Intersection.cpp : Defines the entry point for the console application.
//

#include "stdafx.h"


// Given segment pq and triangle abc, returns whether segment intersects
// triangle and if so, also returns the barycentric coordinates (u,v,w)
// of the intersection point
int IntersectSegmentTriangle(Point p, Point q, Point a, Point b, Point c,
                             float &u, float &v, float &w, float &t)
{
    Vector ab = b - a;
    Vector ac = c - a;
    Vector qp = p - q;

    // Compute triangle normal. Can be precalculated or cached if
    // intersecting multiple segments against the same triangle
    Vector n = Cross(ab, ac);

    // Compute denominator d. If d <= 0, segment is parallel to or points
    // away from triangle, so exit early
    float d = Dot(qp, n);
    if (d <= 0.0f) return 0;

    // Compute intersection t value of pq with plane of triangle. A ray
    // intersects if 0 <= t. Segment intersects if 0 <= t <= 1. Delay
    // dividing by d until intersection has been found to pierce triangle
    Vector ap = p - a;
    t = Dot(ap, n);
    if (t < 0.0f) return 0;
    if (t > d) return 0; // For segment; exclude this code line for a ray test

    // Compute barycentric coordinate components and test if within bounds
    Vector e = Cross(qp, ap);
    v = Dot(ac, e);
    if (v < 0.0f || v > d) return 0;
    w = -Dot(ab, e);
    if (w < 0.0f || v + w > d) return 0;

    // Segment/ray intersects triangle. Perform delayed division and
    // compute the last barycentric coordinate component
    float ood = 1.0f / d;
    t *= ood;
    v *= ood;
    w *= ood;
    u = 1.0f - v - w;
    return 1;
}

int main(int argc, char** argv)
{
	return 0;
}
```