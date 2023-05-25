This documentation provides an overview of normal maps in Unreal Engine 4 (UE4) and their usage within the context of Tekken 7 modding. It explains the specific considerations and techniques required to correctly apply normal maps to characters and stage materials in Tekken 7.

## Standard Normal Maps
In most cases, standard normal maps in UE4 are defined as `Normalmap (DXT5, BC5 on DX11)`, which utilizes the DXT5 or BC5 compression formats on DX11. These normal maps accurately represent surface details and are commonly used in materials such as those found in stage environments.

## Character Materials in Tekken 7
When dealing with character materials in Tekken 7, a different approach is required. The character materials in Tekken 7 utilize the alpha channel of the normal map texture as the roughness parameter. Therefore, the `Normalmap (DXT5, BC5 on DX11)` format cannot be used directly, as it may lead to unintended visual artifacts.

Tekken 7 character materials require the use of DXT5 format with the sRGB option turned off. It is crucial to disable sRGB to avoid undesired visual effects, such as a red glow during character rage or a blue glow when performing a rage drive.

## Stage Materials in Tekken 7
Normal map textures for stage materials can use the `Normalmap (DXT5, BC5 on DX11)` compression format since stage materials don't do anything special with the normal map other than it's intended purpose.