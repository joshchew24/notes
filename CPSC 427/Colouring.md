# Colouring
> [!info] See Also
> [[Uniform Variables]]

Declare the vec and use it in our fragment shader
```clike
#glsl
uniform vec4 ourColor;
fragColor = ourColor;
```

Locate the uniform variable in C++ and pass the desired values into the shader
```cpp
unisigned int ourColor = glGetUniformLocation(m_shaderProgram, "ourColor");
glm::vec4 color = glm::vec4(0.0, 0.0, 0.0, 1.0); // black
glUniform4fv(ourColor, 1, glm::value_ptr(color));
```