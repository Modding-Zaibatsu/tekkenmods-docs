This documentation entry provides guidelines and instructions for modding Tekken 7, which is built on the Unreal Engine 4. It is important to note that modding Tekken 7 follows the same principles as modding any other Unreal Engine 4 game, with the exception that Tekken 7 requires a specific UE4 build.

## Modding Overview
Modding in UE4 involves replacing or modifying game assets such as textures, meshes and levels. By leveraging the modular structure of UE4 games, mods can introduce new content, alter gameplay mechanics, or replace existing meshes.

## Loading Assets in UE4
UE4 employs a virtual file system (VFS) that handles asset loading and management. The VFS is responsible for locating and loading assets based on their file paths and names. When a game starts, the engine scans predefined directories to find and load the required assets.

## Asset Path Structure
> *Read Also: [Understanding the Game Files](../../Basic_Information/Understanding_the_Game_Files/)*

The game developers organize assets into a hierarchical structure based on their types and categories. Each asset resides in a directory corresponding to its type. For example, character content are stored in the `TekkenGame/Content/Characters` directory, stage assets in the `TekkenGame/Content/Stage` directory, and so on.

## Asset Loading Process
When a game is launched, UE4 loads assets dynamically as they are required during gameplay. The engine searches for assets based on their file paths and names within the uasset file that references them.

## Modding with .pak Files
> *Read Also: [Packaging Mod Files](../../Basic_Information/Packaging_Mod_Files/)*

Modders can package their assets into .pak files and place them in the `~mods` directory. These .pak files contain the modified assets that the engine can load during runtime. Here's an overview of the process:

1. Create a .pak File: Gather the modified assets and package them into a .pak file using UnrealPak or other tools like u4pak.

2. Maintain File Path and Name: Ensure that the modified assets have the same file paths and names as the original assets both in the UE4 project and inside the pak file. This allows the engine to locate and replace the assets seamlessly.

3. Place the .pak File: Copy the created .pak file to the game's ~mods folder, typically located in the game's root directory.

4. Mod Activation: When launching the game, the engine recognizes the .pak files in the ~mods folder and loads the assets they contain. The modded assets will replace their counterparts in the game, providing the desired modifications.

The game can also load custom uasset file.

## Guides

- [Setting Up UE4 on Windows](Setting_Up_UE4_on_Windows/)
    - This guide will guide you through the process of setting up a specific version of Unreal Engine 4 on Windows.

- [Setting Up UE4 on Linux](Setting_Up_UE4_on_Linux/)
    - This guide will guide you through the process of setting up a specific version of Unreal Engine 4 on Linux.