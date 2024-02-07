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

```
4. Modify shaders to use texture
	- vertex for position
	- fragment for colors
6. Draw triangles