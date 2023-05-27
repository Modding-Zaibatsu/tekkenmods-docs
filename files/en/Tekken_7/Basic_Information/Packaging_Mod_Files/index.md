## Introduction
This documentation guide explains the process of packaging Unreal Engine 4 uasset files for Tekken 7 using the UnrealPak tool. UnrealPak is a command-line utility provided by Unreal Engine that allows you to create packaged files from your game assets. This guide will cover two methods: using the provided batch files and using the command line tool directly.

## Prerequisites
Before proceeding, ensure that you have the following:

- UnrealPak: Download the UnrealPak utility by obtaining the [UnrealPak.zip](UnrealPak.zip) file, which includes the following files:
    - `UnrealPak.exe`: The main executable for UnrealPak.
    - `UnrealPak-With-Compression.bat`: A batch file for packaging assets with compression.
    - `UnrealPak-Without-Compression.bat`: A batch file for packaging assets without compression.
    - `UnrealUnpak.bat`: A batch file for unpacking pak files.

- Uasset files: Make sure you have the necessary cooked uasset files. Ensure that the files are in the appropriate directory structure required by the game.

## Method 1: Using the Provided Batch Files

1. Extract the contents of `UnrealPak.zip` to a convenient location on your computer.

2. Open the directory containing the extracted files, and locate the `UnrealPak-With-Compression.bat` or `UnrealPak-Without-Compression.bat` file based on your desired compression method.

3. Drag and drop your uasset files or the directory containing them onto the respective batch file (`UnrealPak-With-Compression.bat` for compression, `UnrealPak-Without-Compression.bat` for no compression).

4. The batch file will process the files and create a packaged file with the same name as the directory you dragged onto it (e.g., `MyMod_P`).

5. The packaged file, containing the uasset files, will be generated in the same directory as the batch file.

## Method 2: Using the Command Line Tool
1. Open the command prompt on your computer.

2. Navigate to the directory where you extracted the contents of `UnrealPak.zip`.

3. To package the uasset files, use the following command:
```bat
UnrealPak-With-Compression.bat <path_to_uasset_files>
```

Replace `<path_to_uasset_files>` with the path to the directory containing your uasset files, and `<output_package_name>` with the desired name for your packaged file.

Example:
```bat
UnrealPak-With-Compression.bat C:\MyMod_P
```

4. Press Enter to execute the command. UnrealPak will process the uasset files and create the specified packaged file (e.g., MyMod_P.pak).

5. The packaged file will be generated in the same directory where the UnrealPak executable is located.