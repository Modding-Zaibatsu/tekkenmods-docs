Asset swapping is the process of swapping asset A with asset B. This is useful when you want to replace one character's item with a different character item. For example: Swapping Jin's 1P upper with Kazuya's jacket upper so that Jin will have Kazuya's jacket instead of his default 1P.

## Swapping with UassetRenamer
UassetRenamer is a command-line tool that allows you to swap Unreal Engine 4 (UE4) uasset files for Tekken 7. This documentation provides instructions on how to use UassetRenamer to perform asset swapping and file path replacement within an asset.

## Installation
To use UassetRenamer, follow these steps:

1. Download the UassetRenamer tool by [clicking here](UassetRenamer.zip).
2. Unzip the downloaded file to a directory of your choice.

## Swapping a Whole Asset
This method is used to swap a complete asset with another asset. The original asset will be replaced by a new asset that retains the name of the original but contains the data from the new asset. To perform this operation:

1. Double-click the UassetRenamer.exe file to launch the tool.
2. A window titled "Open reference file" will appear. Navigate to and select the asset file you want to replace, then click "Open."
3. A new window titled "Open file to change id of" will open. Navigate to the asset file you want to use as the replacement and click "Open."
4. The tool will generate a new asset file in the same directory as UassetRenamer. This new file will have a suffix of " -new," indicating that it is a newly created asset.

## Swapping a File Path within an Asset
This method allows you to replace a specific file path within an asset. Follow these steps:

1. Open the asset file using an asset viewer such as CompareIt.
2. Locate the file path within the asset that you wish to replace.
3. Rename the asset file to match the name of the file you want to replace.
4. Launch UassetRenamer by double-clicking the UassetRenamer.exe file.
5. A window titled "Open file to change id of" will appear. Navigate to the asset file you wish to replace and click "Open."
6. Another window titled "Open file to change id of" will open. Navigate to the asset file you renamed in Step 3 and click "Open."
7. The tool will generate a new asset file in the same directory as UassetRenamer. This new file will have a suffix of " -new," indicating that it is a newly created asset.
8. Rename the "new" asset file to the name of your original asset.

## Command-Line Method
UassetRenamer can also be used via the command line. Here is the syntax:

```bat
UassetRenamer [reference file] [file to change id of]
```

The tool expects two arguments:

- `[reference file]`: This is the path to the reference file, which serves as a template for the new filename.
- `[file to change id of]`: This is the path to the file that needs to be renamed.

By specifying these arguments, you can process the files using UassetRenamer for the renaming operation.

**Example:**
```bat
UassetRenamer C:\Assets\ReferenceAsset.uasset C:\Assets\AssetToRename.uasset
```

This command will use `ReferenceAsset.uasset` as a template and rename `AssetToRename.uasset` accordingly.