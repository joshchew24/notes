# Vertex Buffer Object (VBO)
- buffer object containing the vertices to pass through OpenGL
```cpp
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*) 0);
glEnableVertexAttribArray(0);
```
- 0 is location
- 3 is the number of values (how many dimensions in the vector)
- `GL_FLOAT` is data type in the vector
- `GL_FALSE` flag to normalize the values (for ints)
- `3 * sizeof(float)` stride through the buffer 
	- each vector has 3 floats, so our stride is 3 times the size of a float
- `(void *) 0` 
## Remderomg
```cpp
// 0. copy our vertices array in a buffer for OpenGL to use
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

// 1. then set the vertex attributes pointers
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*) 0);
glEnableVertexAttribArray(0);

// 2. use our shader program when we want to render an object
glUseProgram(shaderProgram);

// 3. draw the object
glDrawArrays(GL_TRIANGLES, 0, 3);