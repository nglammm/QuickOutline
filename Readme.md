Quick Outline
=============

Developed by Chris Nolet (c) 2018


About
-----

Quick Outline is a world-space outline tool, that adds a solid outline to any object.

It’s ideally suited for VR.

Many outline shaders work in screen space, which makes them slow – and they don’t support MSAA. If they do work in world space, they have ‘gaps’ on hard corners. Quick Outline addresses these issues.

Quick Outline was originally designed for VR, so it supports Instanced Stereo rendering and MSAA. It looks great in any HMD, and it won’t impact the frame rate.

- Designed for VR (including single pass)
- Supports MSAA
- Compatible with post-processing stack
- Multiple outline modes
- Lightweight and performant


Instructions
------------

To add an outline to an object, drag-and-drop the Outline.cs script onto the object. The outline materials will be loaded at runtime.

You can also add outlines programmatically with:

    var outline = gameObject.AddComponent<Outline>();

    outline.OutlineMode = OutlineMode.OutlineAll;
    outline.OutlineColor = Color.yellow;
    outline.OutlineWidth = 5f;

The outline script does a small amount of work in Awake(). For best results, use outline.enabled to toggle the outline. Avoid removing and re-adding the component if possible.

For large meshes, you may also like to enable 'Precompute Outline' in the editor. This will reduce the amount of work performed in Awake().

Changing outline in runtime via external scripts
------------------------------------------------

You can also change the outline while run-time by fetching the 'Outline' component and run the function:

`ApplySettings(OutlineEffect outlineEffect, bool priority = false)`

Definition for `OutlineEffect` parameter:
```
[Serializeable]
public struct OutlineEffect
{
    public OutlineMode outlineMode;
    public Color outlineColor;
    public float outlineThickness;
}
```

`priority` parameter (default `false`) is used to determine whether we change the render queue to `3099` (which is lower than the default `3100` and will be rendered first) so layering will be correct.

Example usage:

```
    OutlineEffect outlineEffect = new();

    // set up the outline effect
    outlineEffect.outlineMode = OutlineMode.OutlineAll;
    outlineEffect.outlineColor = Color.white;
    outlineEffect.outlineThickness = 3;

    // get the outline component
    Outline outline = GetComponent<Outline>();
    outline.ApplySettings(outlineEffect, priority = true); // rendered first
```


Troubleshooting
---------------

If the outline appears off-center, please try the following:

1. Set 'Read/Write Enabled' on each model's import settings.
2. Disable 'Optimize Mesh Data' in the player settings.
