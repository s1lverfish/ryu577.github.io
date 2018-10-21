---
layout: post
comments: true
title:  "Can we see four dimensional space with our own two (dimensional) eyes?"
date:   2017-03-25 16:34:40 -0800
categories: jekyll update
---

This blog is based on [a video][cubes] about slicing a four dimensional cube or Teserract. Check it out.


## Building up to seeing in four dimensions
If we want to visualize a four dimensional phenomenon, we can't just visualize a four dimensional phenomenon. We first need to understand the phenomenon well, see what it looks like in three dimensions. Then, we must know what to expect in four dimensions and then, we will be ready to truly see it in all its glory. So, let's begin the first step of this journey.

## Slicing a cube
The simplest shape in space of any dimensionality is a cube. When we draw a cube in a space, it is partitioned into the region outside the cube and the region inside the cube. The region outside the cube is of course, infinite. But the region inside provides a means to measure the space the cube lives in. Whenever someone draws a closed boundary and asks how much space is within the boundary, we can try and count the number of cubes we would be able to fit into it. 

So, since cubes are such a fundamental property of space, let's try and observe a phenomenon within cubes. It involves slicing the cube. Pick any vertex of the cube and move to its three nearest neighbors (there will be three since we're in 3 dimensional space). Joining these three vertices will result in an equilateral triangle. Then, we move from those to their nearest neighbors. There turn out to be three of those as well and they form another equilateral triangle parallel to the first one. This is best visualized in the figure below - 

![My helpful screenshot]({{site.url}}/Downloads/Hypercubes/cubeNeibors2.gif)

*Figure 1: Start at any vertex. Its nearest neighbors form an equilateral triangle. And if we then go to their nearest neighbors, another equilateral triangle.*

Let's inspect this cube with it's two equilateral triangles more closely - 

![My helpful screenshot]({{site.url}}/Downloads/Hypercubes/bodyDiag4.gif)

*Figure 2: A three dimensional look at the equilateral triangles described above.*

And now, we can go ahead and cut the cube along these two equilateral triangles.

![My helpful screenshot]({{site.url}}/Downloads/Hypercubes/cutCube.gif)

*Figure 3: Slicing the cube along the two saperating planes.*

## Shapes of the slices

The red and blue shapes in figure 3 above look a lot like [Tetrahedra][tetrahedron] while the green shape in the middle looks like an [Octahedron][octahedron], which are the simplest of the [Platonic solids][platonic] (three dimensional objects that have regular polygons like equilateral triangles as their faces). There is however, a small thorn in the side here. If we look at the front faces of the blue and green objects, they are right angled triangles (being part of a cube). Platonic solids like Tetrahedra and Octahedra are composed of only right angled triangles, however. So, these seem to be imperfect versions of the Platonic solids.

![My helpful screenshot]({{site.url}}/Downloads/Hypercubes/Imperfect.jpg)

*Figure 4: Since the two front faces are right angled not equilateral, these shapes seem to be distorted versions of the Platonic solids.*

But, here's another observation - the cutting planes along which we sliced our cube were certainly equilateral. However, when we project the cube to a two dimensional square as in figure 5 below, they look right angled. Could something similar be happening when we project a four dimensional object to this three dimensional cube?

![My helpful screenshot]({{site.url}}/Downloads/Hypercubes/EquilateralToRight.gif)

*Figure 5: Equilateral triangles look like right angled when the cube is projected to a square.*

## Slicing a Teserract

Turns out, this is indeed the case. In figure 6 below, we see a four dimensional cube, also called a Teserract. It has 16 vertices (2^4). And since this is four dimensional space, the corresponding cutting planes formed by the nearest neighbors will be three dimensional (like they were two dimensional when we were in the three dimensional space of the cube). We start with the bottom left point. Since this is a cube in 4d space, the point will have four nearest neighbors. These four points form the blue tetrahedron (which is perfectly Platonic - all its faces are equilateral). Then, those four vertices are connected to six other vertices. These form the green (again, Platonic) Octahedron. and finally, we get the red tetrahedron from the nearest neighbors of the green Octahedron. It is these three shapes we were seeing our 3d cube split into in the previous section. However, the projection to a lower space was causing them to look imperfect (right angled faces).

![My helpful screenshot]({{site.url}}/Downloads/Hypercubes/sliceTeserract.gif)

*Figure 6: Slicing a Teserract. There are three cutting planes - a tetrahedron, octahedron and tetrahedron.*



[cubes]: https://www.youtube.com/watch?v=57g6nQGBFcY
[tetrahedron]: https://en.wikipedia.org/wiki/Tetrahedron
[octahedron]: https://en.wikipedia.org/wiki/Octahedron
[platonic]: https://en.wikipedia.org/wiki/Platonic_solid

