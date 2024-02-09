# Texture Mapping
> [!danger] Texture origin (0,0) is in the bottom left, not the center like OpenGL and ranges from 0 to 1

1. Create a texture variable and bind to OpenGL
```cpp
glGenTextures(1, &m_dirt_texture); // create a texture id
glBindTexture(GL_TEXTURE_2D, m_dirt_texture);
```
2. Load texture file into OpenGL
```cpp
int w, h, nChannels;
unsigned char* data = stbi_load("..//..//..//data//textures//dirt_grass.png", &w, &h, &nChannels, 0);

if (data) {
	glTexImage2D(GL_TEXTURE_2D, 0, GL_RGBA, w, h, 0, GL_RGBA, GL_UNSIGNED_BYTE, data);
	glGenerateMipMap(GL_TEXTURE_2D);
} else {
	std::err << "ERROR: failed to load texture" << std::endl;
}
stbi_image_free(data);
```
3. Map texture to vertices
```cpp
// modify our vertices to be a square
// x, y, z,    s, t
float vertices[] = {
	0.5f, 0.5f, 0.0f,    1.0f, 1.0f,      // top right
	0.5f, -0.5f, 0.0f,   1.0f, 0.0f,      // bottom right
	-0.5f, 0.5f, 0.0f,   0.0f, 1.0f       // top left
};
// change the stride value
unsigned int stride = (3 + 2) * sizeof(float);
glVertexAttributePointer(1, 2, GL_FLOAT, GL_FALSE, stride, (void*)(3 * sizeof(float));
EnableVertexAttribArray(1);
```
4. Modify shaders to use texture
- vertex for position
```clike
#version 330 core
layout (location = 0) in vec3 aPos;
layout (location = 1) in vec2 aTexCoord;
uniform mat4 transform;
out vec2 TexCoord;     // need to pass through to fragment shader
void main()
{
   gl_Position = transform * vec4(aPos, 1.0);
   TexCoord = aTexCoord;
}
```
- fragment for colors
```clike
#version 330 core
out vec4 fragColor;
uniform vec4 ourColor;
in vec2 TexCoord;
uniform sampler2D ourTexture;
void main()
{
   // fragColor = vec4(1.0f, 0.0f, 0.0f, 1.0f);
   // fragColor = ourColor;
   fragColor = texture(ourTexture, TexCoord);
}
```
5. Draw triangles