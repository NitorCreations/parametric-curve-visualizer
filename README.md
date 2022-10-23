# parametric-curve-visualizer

Visualize parametric curves.

There are plenty of tools to visualize parametric curves. However the idea is not only to visualize them, but to also make them look beautiful. Beautiful, smooth tubes floating in space.

The visualizer is limited to sine and cosine functions, mainly because if you want to have a pretty looking curve then it should form a loop. So periodic functions make sense, and thus sin and cos are the natural simplest choices.

In theory, any curve could be visualized if you are just able to calculate the derivatives of the expressions for the x, y and z coordinates. For sin and cos this is particularly easy.

### How it works

Mathematically, the tool is quite simple. The mesh to be rendered is a simple square from (-1, -1, 0) to (1, 1, 0) and subdivided to several squares in both x and y directions (you can specify how many). The vertex shader then transforms this flat square so that the x direction goes along the curve, and the y direction wraps the square around the curve, to form a nice-looking curved tube.

The points on the curve we can determine trivially from the definition of the curve itself. In order to draw a tube around the curve we first find the tangent of the curve at the point of the curve that was computed previously. This is also rather simple because the expression of the curve consists of only sin and cos functions added together. The tangent vector is computed simply by computing the derivatives of the curve expression for x, y and z at the point of the curve. The tangent vector then has to be normalized.

The next step after finding the (unit) tangent vector is to find another vector perpendicular to it, in order to compute how the tube wraps around the curve. We can compute one such vector with a cross-product of the position vector and the tangent vector. It then also has to be normalized. Finally we can compute a third vector that is perpendicular to both to the tangent vector and the vector just calculated, as their cross product. These two vectors last calculated can then be used to compute the vertex locations for the circle that is on the surface of the tube that is wrapped around the curve (using sin and cos and those two vectors as perpendicular unit vectors).

Surface normals are also computed only on the fly, they are actually the same vector that specifies the vertex location on the tube (relative to the curve).

[Try it!](https://raw.githack.com/NitorCreations/parametric-curve-visualizer/main/webgl.html) Scroll down to adjust the parameters of the curve.
