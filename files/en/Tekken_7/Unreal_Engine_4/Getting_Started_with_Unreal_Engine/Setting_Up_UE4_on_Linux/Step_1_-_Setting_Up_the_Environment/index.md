**INFO**: This guide series assumes that you are familiar with Linux.

Since we are dealing with an old engine build, running UE4's build scripts on newer distros are likely to fail due to missing required packages and variations between different Linux distributions.

It's recommended to set up a virtual environment where Unreal Engine 4 can be compiled.

## The Virtual Machine Approach
In this series we will use [Ubuntu 16.04 Server](https://releases.ubuntu.com/16.04/) to compile Unreal Engine 4. To get started, boot your preferred virtualization software. QEMU/KVM is recommended as it allows you to mount a physical drive and dedicate a set amount of CPU cores for the guest machine.

Install Ubuntu 16.04 Server like you would any other distro and once the installation process is completed log into your user.

## Cloning and Setting Up UE4
We will clone the [Modding-Zaibatsu/ue4-tekkengame](https://github.com/Modding-Zaibatsu/ue4-tekkengame) repository by running the command:
```bash
~ git clone https://github.com/Modding-Zaibatsu/ue4-tekkengame.git
```

This will create a folder called "ue4-tekkengame" in your current working directory and download all the files from the repository into it.

We will then navigate to the repo directory.

```bash
~ cd ue4-tekkengame
```

**WARNING**: Make sure that the UE4 files aren't located in a path that contains spaces.

We will then set the permission of two script files to allow it's execution

```bash
~/ue4-tekkengame sudo chmod +x Setup.sh
~/ue4-tekkengame sudo chmod +x Engine/Build/BatchFiles/Linux/Setup.sh
```

After that, we can run `Setup.sh`

```bash
~/ue4-tekkengame ./Setup.sh
```
The script will now download and install the required files for Unreal Engine 4. Once the script finished executing without any errors, we'll need to generate the project files for the engine.

```bash
~/ue4-tekkengame ./GenerateProjectFiles.sh
```

After the project files were generated, you're ready to compile the engine by running the `make` command.

```bash
~/ue4-tekkengame make
```