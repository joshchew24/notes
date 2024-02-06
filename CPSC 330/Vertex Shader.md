# Vertex Shader
## Minimal Shader
```clike
# glsl
# version 330 core
layout (location = 0) in vec3 pos;

void main()
{
	gl_Position = vec4(pos.x, pos.y, pos.z, 1.0);
}
```
- `in` keyword indicates an input variable
- `vec3` is the data type
- `pos` is the name of the variable

```python
if __name__ == "__main"":
	main()
```

