Once you've set up and compiled Unreal Engine 4, you are ready to compile the TekkenGameNative project.

Clone the [TekkenGameNative repository](https://github.com/Modding-Zaibatsu/TekkenGameNative) by running the command:
```bash
~ git clone https://github.com/Modding-Zaibatsu/TekkenGameNative.git
```

Make sure you're not cloning into the Unreal Engine 4 directory.

Enter the TekkenGameNative directory:
```bash
~ cd TekkenGameNative
```

Since we can't generate the project files from the Unreal Editor itself, we will have to do it via the GenerateProjectFiles script in Unreal Engine 4's directory.

```shell
~/TekkenGameNative bash /path/to/ue4/GenerateProjectFiles.sh -project="TekkenGame.uproject" -game
```

Once the script is done generating the project files, run the following:
```shell
~/TekkenGameNative make TekkenGameEditor
```

This will compile the TekkenGameNative project.