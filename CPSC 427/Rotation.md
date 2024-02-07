# Rotation
## Rotation around the Z-axis:
$$
\begin{bmatrix}
cos(\theta) & -sin(\theta) & 0 & 0 \\
sin(\theta) & cos(\theta) & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
\cdot
\begin{pmatrix}
x \\ y \\ z \\ 1
\end{pmatrix}
=
\begin{pmatrix}
cos(\theta)\cdot x-sin(\theta)\cdot y \\
sin(\theta)\cdot x+cos(\theta)\cdot y \\
z \\
1
\end{pmatrix}
$$

```cpp
// matrix rotation (around Z)
float rotation = 90.0f;
// start with an identity matrix
glm::mat4 trans = glm::mat4(1.04f);
// pass the identity matrix, pass the rotation converted to radians, pass the vector being rotated
trans = glm::rotate(trans, glm::radians(rotation), glm::vec3(0.0, 0.0, 1.0));
```
## Vertex Shader
We must apply the transformation to the positiosn of the vertices
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