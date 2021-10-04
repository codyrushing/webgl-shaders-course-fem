# webgl-shaders-course-fem
* Course: https://frontendmasters.com/courses/webgl-shaders/
* Course repo: https://github.com/mattdesl/workshop-webgl-glsl
  * mostly just a collection of resources and cheatsheets
* Interactive slides: https://mattdesl.github.io/workshop-webgl-glsl/#/
* Helpful Three demos: https://three-demos.glitch.me/

## Intro
* WebGL is really just a way to get at the GPUs parallel processing.
* GLSL is a little program that gets compiled directly for the GPU and can run very fast
* WebGL is often used for rendering applications in real-time, but you could also use it to generate complex geometries that you can export and use later

#### canvas-sketch
* `npx canvas-sketch sketches/00-basic.js --new --template=three`
  * scaffolds out a new three.js sketch to that file

## Three.js intro

#### Coordinate systems
* Typically coordinate system has the origin (0,0) at the top left most point of the canvas
* There is also the _cartesian_ system which places the origin (0,0) at the center.  __This is also usually what is used in a 3D application__

#### GPGPU (general purpose GPU)
* Using the GPU to do arbitrary calculations, not necessarily for rendering.  

* By default, only one side of a material in Three is visible.  You have to set `side: THREE.DoubleSide` in the material constructor for it to be double sided

#### Custom geometries
* You can manually declare vertices for a shape, define faces from those vertices, and render them to a mesh.
* You can also use a BufferGeometry, which is good when you maybe have a complex data set that is not in some sort of 3D-ready format (like some generic quantitative data).  BufferGeometry will allow us to pass in a large amount of data in a flat Float32Array format and it will create all of the vertices, faces, and everything else.  It would not work to pass custom positional data to the other standard Geometry classes since, you would need to either build your own custom Geometry or a BufferGeometry.

## Shaders
Fragment shaders can be used to manipulate the surface quality of a mesh.  Their purpose is to determine the color of each pixel in something - sometimes the whole screen, sometimes just a mesh.  Fragment shaders do not go sequentially through all the pixels and run the shader program - they run the shader program for all of them __in parallel__.  This means that you can't reference pixel A in the shader for pixel B, because they're all being drawn at the same time.

Vertex shaders determine the screen-space position of each vertex in a mesh.  This is to say that it determines where in a 2D space each vertex in a 3D object should show up, kinda like a map projection.  Vertex shaders are also parallel, so each vertex can't know about each other.  

```glsl
precision highp float;

uniform vec3 color;
uniform float opacity;

void main () {
  gl_FragColor = vec4(color, opacity);
}
```

A `uniform` is a value that is passed in from JS.  It is called a uniform because it is the same for every pixel in the fragment shader (or vertex in a vertex shader).  It is not changeable for a given render.  You would use a uniform to pass data from some interactive JS element to the GLSL shader.

There are also `attribute` variables which can be unique to the pixel/vertex.  An example of one would be the position of a vertex in a mesh, that would get passed to the shader as an attribute.

When using a Three.js shader, you get some built in uniforms `projectionMatrix` and attributes `position` and `uv`.

#### The rendering pipeline
* First there is data for a geometry (in our case, from a Three.js geometry). This defines any `attribute` variables that come from the geometry, and the JS caller can define any `uniform` variables.
* That gets passed to the vertex shader, which determins vertices.  Vertex shaders can also define any `varying` values that can be passed into the fragment shader.
* Those vertices can be passed to the fragment shader which does the real "rendering".  