# webgl-shaders-course-fem
* Course: https://frontendmasters.com/courses/webgl-shaders/
* Course repo: https://github.com/mattdesl/workshop-webgl-glslnpm
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
* You can also use a BufferGeometry, which is good when you maybe have a complex data set that is not in some sort of 3D-ready format (like some generic quantitative data).  BufferGeometry will allow us to pass in a large amount of data in a flat Float32Array format and it will create all of the vertices, faces, and everything else.  It would not work to pass custom positional data to the other standard Geometry classes since, you would need to either build your own custom Geometry or a BufferGeometry