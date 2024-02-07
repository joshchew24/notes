# Translation
$$
\begin{bmatrix}
1 & 0 & 0 & T_x \\
0 & 1 & 0 & T_y \\
0 & 0 & 1 & T_z \\
0 & 0 & 0 & 1
\end{bmatrix}
\cdot
\begin{pmatrix}
x \\ y \\ z \\ 1
\end{pmatrix}
=
\begin{pmatrix}
x + T_x \\
y + T_y \\ 
z + T_y \\
1
\end{pmatrix}
$$
```cpp
// matrix translate
float t_x, t_y, t_z;
// assign values to t_x, t_y, t_z
glm::mat4 trans = glm::mat4(1.0f);
trans = gLm::translate(trans, glm::vec3(t_x, t_y, t_z)); 
```