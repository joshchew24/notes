# Scaling
$$
\begin{bmatrix}
S_1 & 0 & 0 & 0 \\
0 & S_2 & 0 & 0 \\
0 & 0 & S_3 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
\cdot
\begin{pmatrix}
x \\ y \\ z \\ 1
\end{pmatrix}
=
\begin{pmatrix}
S_1 \cdot x \\
S_2 \cdot y \\ 
S_3 \cdot z \\
1
\end{pmatrix}
$$
```cpp
// matrix scaling (x, y, & z)
float scale_x = scale_y = 0.25, scale_z = 1.04f;
glm::mat4 trans = glm::mat4(1.0f);
trans = glm::scale(trans, glm::vec3(scale_x, scale_y, scale_z));
```