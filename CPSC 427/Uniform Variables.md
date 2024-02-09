# Uniform Variables
- Shader "uniform" variables are **global** to the shader program
- can be passed to either the vertex or fragment shader after being loaded into the program
## Process
1. Declare the uniform variable in the shader
```clike
uniform mat4 transform;
```
2. Use it in the shader
```clike
gl_Position = transform * vec4(aPos, 1.0);
```
3. Locate the variable in your C++ program
```cpp
tLoc = glGetUniformLocation(m_shaderProgram, "transform");
```
4. Pass the data into the shader (type specific)
```cpp
glUniformMatrix4fv(tLoc, 1, GL_FALSE, glm::value_ptr(trans));
```
