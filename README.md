Low Level Graphics Library (LLGL)
=================================

Overview
--------

- **Version**: 1.00 Alpha
- **License**: [3-Clause BSD License](https://github.com/LukasBanana/LLGL/blob/master/LICENSE.txt)


Progress
--------

- **OpenGL Renderer**: ~70% done
- **Direct3D 12 Renderer**: ~5% done
- **Direct3D 11 Renderer**: not started yet
- **Vulkan Renderer**: not started yet


Getting Started
---------------

```cpp
#include <LLGL/LLGL.h>

int main()
{
	// Create a window to render into
	LLGL::WindowDescriptor windowDesc;

	windowDesc.title    = L"LLGL Example";
	windowDesc.visible  = true;
	windowDesc.centered = true;
	windowDesc.width    = 640;
	windowDesc.height   = 480;

	auto window = LLGL::Window::Create(windowDesc);

	// Add keyboard/mouse event listener
	auto input = std::make_shared<LLGL::Input>();
	window->AddEventListener(input);

	//TO BE CONTINUED ...

	// Main loop
	while (window->ProcessEvents() && !input->KeyPressed(LLGL::Key::Escape))
	{
		
		// Draw with OpenGL, or Direct3D, or Vulkan, or whatever ...
		
	}
	
	return 0;
}
```


Thin Abstraction Layer
----------------------
```cpp
// Interface:
RenderContext::DrawIndexed(unsigned int numVertices, unsigned int firstIndex);

// OpenGL Implementation:
void GLRenderContext::DrawIndexed(unsigned int numVertices, unsigned int firstIndex)
{
	glDrawElements(
		renderState_.drawMode,
		static_cast<GLsizei>(numVertices),
		renderState_.indexBufferDataType,
		(reinterpret_cast<const GLvoid*>(firstIndex * renderState_.indexBufferStride))
	);
}

// Direct3D 11 Implementation
void D3D11RenderContext::DrawIndexed(unsigned int numVertices, unsigned int firstIndex)
{
	context_->DrawIndexed(numVertices, 0, firstIndex);
}

// Direct3D 12 Implementation
void D3D12RenderContext::DrawIndexed(unsigned int numVertices, unsigned int firstIndex)
{
	LLGL_D3D_ASSERT(gfxCommandList_)->DrawIndexedInstanced(numVertices, 1, firstIndex, 0, 0);
}

// Vulkan Implementation
void VKRenderContext::DrawIndexed(unsigned int numVertices, unsigned int firstIndex)
{
	vkCmdDrawIndexed(commandBuffer_, numVertices, 1, firstIndex, 0, 0);
}
```


