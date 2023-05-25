The Unreal Engine 4 BC7 texture format is a widely used texture compression format within the Unreal Engine game development framework. BC7, also known as Block Compression 7, provides efficient compression for high-quality textures. It produces better quality than the more widely available DXT5 format, but it requires a modern GPU and is it's compression process is often slower compared to other formats.

**NOTE**: BC7 should be used for any textures that need high quality compression.

## Usage Considerations for Tekken 7 Modding
When working with BC7 textures in the context of Tekken 7 modding, it's important to keep the following considerations in mind:

1. **Tekken 7 Texture Editor Limitations**: It's worth noting that the Tekken 7 Texture Editor does not support importing or exporting BC7 textures directly. Alternative methods are required to work with BC7 textures for modding purposes.

2. **Exporting BC7 Textures**: BC7 textures can be exported from the game files using tools like "umodel." This tool allows extraction of game assets, including BC7 compressed textures.

3. **Importing BC7 Textures**: To import BC7 textures into the game, it is necessary to cook a texture within Unreal Engine 4 (UE4) and set its compression format to BC7.

## Working with BC7 Textures in Unreal Engine 4
When working with BC7 textures in UE4, the following considerations apply:

1. Normal Maps: In Tekken 7, normal maps for characters do not follow the conventional `Normalmap (DXT5, BC5 on DX11)` format. Instead, the character normal maps use DXT5 with sRGB turned off since the game developers use the alpha channel as the roughness. As a result, formats like BC7 can be used as a normal map format in Tekken 7. However, it is crucial to toggle off the sRGB option to avoid unintended glowing effects when characters enter rage mode.

2. `sRGB Toggle`: Depending on the specific usage, it is essential to enable or disable the sRGB option for BC7 textures in UE4. For UI textures, where color accuracy is paramount, sRGB should be turned off. For normal maps, the alpha channel is used as the roughness map so sRGB needs to be turned off as well. For other texture types, it is necessary to determine whether sRGB should be enabled or disabled based on the desired visual outcome and the texture's intended purpose.

## See Also

- [`Getting Started with Unreal Engine`](../Unreal_Engine_4/Getting_Started_with_Unreal_Engine/)
- [`UModel - Unreal Engine Model Viewer`](https://www.gildor.org/en/projects/umodel)
- [`Normal Maps`](../Normal_Maps/)