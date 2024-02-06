# Fragment Shader
```clike
#version 330 core
out vec4 fragColor;

void main()
{
	fragColor = vec4(1.0f, 0.0f, 0.0f, 1.0f);   // RGBa
}
```
- `out` means we are outputting this variable
## Compiling
```cpp
std::string shaderSource = <shader-code>;    // <shader-code> is where the shader code is
const char *c_str = shaderSource.c_str()     // OpenGL is C, so we have to use char array 

// create a shader object
GLuint vertexShader;
vertexShader = glCreateShader(GL_FRAGMENT_SHADER);

// 1 = # of strings
glShaderSource(vertexShader, 1, & c_str, NULL);
glCompileShader(vertexShader);
```
## Link
```cpp
Gluint shaderProgram;
shaderProgram = glCreateProgram();

// attached & link compiled shaders
glAttachShader(shaderProgram, vertexShader);
glAttachShader(shaderProgram, fragmentShader);
glLinkProgram(shaderProgram);

// clean up
glDeleteShader(vertexShader);
glDeleteShader(fragmentShader);