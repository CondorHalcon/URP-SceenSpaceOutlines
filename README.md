# Unity Screen Space Outlines

I was working on the 2024 AIE Minor Production project [*Splash & Seek*](https://aieseattle.itch.io/splashseek) as lead programmer and technical artist. As part of the art style, the team needed an implementation for global outlines in the Unity engine. It then became my task to find an appropriate solution.

#### Considerations
1. Consistent Outlines: Outlines need to be a consistent size across all objects.
2. Easy tweaking: Outline parameters like color and size need to be easily tweaked by artists and designers.

## 1. First Method | Outline Material

#### The Implementation
#### The Problems
#### The Result
#### Potential Improvements

## 2. Final Method | URP Render Feature

#### The Implementation
> Draws screen space outlines along edges. The edge detection uses view normals and screen depth.

```csharp
namespace CondorHalcon.URPRenderFeatures
{
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
    }
}
```

#### The Problems
#### The Result
#### Potential Improvements
