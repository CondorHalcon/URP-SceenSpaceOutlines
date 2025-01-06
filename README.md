# URP Screen Space Outlines

I was working on the 2024 AIE Minor Production project [*Splash & Seek*](https://aieseattle.itch.io/splashseek) as lead programmer and technical artist. As part of the art style, the team needed an implementation for global outlines in the Unity engine. It then became my task to find an appropriate solution.

#### Considerations
1. Consistent Outlines: Outlines need to be a consistent size across all meshes.
2. Easy tweaking: Outline parameters like color and size need to be easily tweaked by artists and designers.

## 1. First Method | Outline Material

#### The Implementation
The first implementation was a simple unlit shader with vertex displacement that only rendered back-faces. By adding a material using the shader to an extra slot on the object's mesh renderer, a false outline was added to the object.

#### The Problems
As this is a fairly simple solution, there were no significant problems while building this; however, there were some problems with the result.

#### The Result
It was easy to test the results of this implementation. Outlines were clear, predictable and consistent. It did not however meet the requirements of the project.

The first problem with this method came from the vertex displacement; on hard edges, the vertex displacement would cause breaks in the mesh where it wasn't intended.

Second problem, there was no easy way for artists to apply the outlines on multiple objects. Due to the nature of this solution, every mesh that needed outlines had to individually apply the outline. While easy, adding this to every single mesh would be a mundane, repeating task that could very easily be missed on some objects. 

## 2. Final Method | URP Render Feature

#### The Implementation
This solution utilizes Unity's Scriptable Render Pipeline by implementing a render feature; more specifically a screen space outline render feature. The reader feature would read view normals and screen depth to compute edges and then draw screen space outlines.

```csharp
public class ScreenSpaceOutlines : ScriptableRenderFeature
{
    // normal texture settings
    private class ViewSpaceNormalsTextureSettings {}
    // outline settings
    private class ScreenSpaceOutlinesSettings {}
    // normal texture pass
    private class ViewSpaceNormalsTexturePass : ScriptableRenderPass {}
    // outline pass
    private class ScreenSpaceOutlinesPass : ScriptableRenderPass {}

    public override void Create() {}
    public override void AddRenderPasses(ScriptableRenderer renderer, ref RenderingData renderingData) {}
}
```

#### The Problems
The biggest problem faced when implementing this solution was the lack of clear documentation on how to use the SRP render features. Online resources were not much help either as many had been outdated by major changes to the SRP API. In the end this significantly slowed down development time as trial and error prove to be the most reliable way to find solutions.

#### The Result
The result met the requirements of the project. There were problems with consistency, as sometimes the feature would not enable until you reloaded the scene, and the order render features were solved affected what objects used the feature. But it worked as intended. Outlines could be enabled and disabled on a entire layers making it easier to tweak what had outlines. They was a single settings panel on the renderer making it easy for artists and designers to tweak what the outlines looked like.

#### Potential Improvements
1. Edge detection: There are minor problems with the edge detection of the render feature. It is currently unclear to me wether the fault is in the shaders, unity settings, or feature code.
2. Settings clarity: While the render pass settings work as intended, it is unclear to the user what is intended. The package would greatly benefit from editor tooltips.
