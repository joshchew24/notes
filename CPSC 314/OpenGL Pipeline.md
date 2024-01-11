# OpenGL Pipeline
- shapes are "discretized" into primitives
	- _triangles_, line segments
	- triangles are defined by positions of their vertices
![[Pasted image 20240110201649.png]]
- vertices are stored in a vertex buffer
- when a draw call is issued, each vertex passes through the vertex shader
- on input to the vertex shader, each vertex has associated attributes
- on output, each vertex has a value for gl_Position
	- and for its varying variables (in WebGL 2, called "out/in")
## Rasterization
![[Pasted image 20240110201948.png]]
- data in gl_Position used to place three vertices on a virtual screen
- rasterizer figures which pixels are inside the triangle
	- interopolates the varying variables from the vertices to each of these pixels
## Fragment Shader
![[Pasted image 20240110202016.png]]
- each pixel is passed through fragment shader
	- computes the final colour of the pixel
- pixel is then placed in framebuffer for display
- changes to the fragment shader can simulate light reflecting off different kinds of materials