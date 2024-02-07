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
