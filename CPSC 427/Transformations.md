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