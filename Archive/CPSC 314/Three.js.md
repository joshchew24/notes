# Three.js
- high level library that can use WebGL for rendering
	-  can also use basic HTML5 canvas for simple things
- easier setup than WebGL
- implements "scene" and "mesh" abstractions
	- mesh $\approx$ geometry + material properties
		- this definition is non-standard
	- scene contains a hierarchy of mesh objects
		- render using Camera