# Transformations
## [[Rotation]]
## [[Scaling]]
## [[Translation]]
## Vertex Shader
We must apply the transformation to the positions of the vertices
```clike
# glsl
# version 330 core
layout (location = 0) in vec3 pos;

uniform mat4 transform;

void main()
{
	gl_Position = transform * vec4(pos.x, pos.y, pos.z, 1.0);
}
```
How do we pass the transformation matrix into the shader program?
```cpp
void GLRender::render(GLWindow& window, const glm::mat4& trans) {
	// send our transform to the shader! (m_shaderProgram)
	unsigned int transformLoc = glGetUniformLocation(m_shaderProgram, "transform");
	# 1 = count; GL_FALSE = transpose
	glUniformMatrix4fv(transformLoc, 1, GL_FALSE, glm::value_ptr(trans));

	// draw our triangles
	glUseProgram(m_shaderProgram);
	glBindVertexArray(m_VAO);
	glDrawArrays(GL_TRIANGLES, 0, 3);
}
```

> [!info] See Also
> [[Uniform Variables]]


## Combining Transformations
- mathematically, we can combine transformations by using the dot-product of the transformations we wish to apply
	- order matters
		- multiplications are applied right to left
			- i.e. the matrix closest to the target vector dgets applied first
		- **first scale, then rotate, finally translate**
- with OpenGL, we just subsequently apply transformations to the same matrix
```cpp
// matrix rotation, scaling, and translating
glm::mat4 trans = glm::mat4(1.0f);
trans = glm::rotate(trans, glm::radians(rotation), glm::vec3(0.0, 0.0, 1.0));
trans = glm::scale(trans, glm::vec3(0.25, 1.0, 1.0));
trans = glm::translate(trans, glm::vec3(0.0, -0.5, 0.0));
```