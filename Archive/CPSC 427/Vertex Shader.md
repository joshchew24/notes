# Vertex Shader
## Minimal Shader
```clike
# glsl
# version 330 core
layout (location = 0) in vec3 pos;

void main()
{
	gl_Position = vec4(pos.x, pos.y, pos.z, 1.0);
}
```
- `in` keyword indicates an input variable
- `vec3` is the data type
- `pos` is the name of the variable

```python
if __name__ == "__main"":
	main()
```

## Compiling
```cpp
std::string shaderSource = <shader-code>;    // <shader-code> is where the shader code is
const char *c_str = shaderSource.c_str()     // OpenGL is C, so we have to use char array 

// create a shader object
GLuint vertexShader;
vertexShader = glCreateShader(GL_VERTEX_SHADER);

// 1 = # of strings
glShaderSource(vertexShader, 1, & c_str, NULL);
glCompileShader(vertexShader);
```