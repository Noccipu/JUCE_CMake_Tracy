# JUCE_CMake_Tracy

This JUCE project template uses the Tracy profiler to provide a real time and detailed profiling solution tailored for advanced audio application development.

## Setup

### Built with
- Windows 11
- [Visual Studio Code](https://code.visualstudio.com/) (1.91.1)
- [CMake](https://cmake.org/) (4.0.2)
- [Tracy](https://github.com/wolfpld/tracy) (0.12)

### Requirements
- [CMake](https://cmake.org/)
- [Last released version of Tracy](https://github.com/wolfpld/tracy/releases) or *(for Windows only)* compiled Windows x64 binaries

### Install Template
- First, clone the GitHub repository to the location of your choice.
```git
git clone <location/in/your/computer>
```

- The template requires the installation of two submodules ([JUCE](https://github.com/juce-framework/JUCE) and [tracy](https://github.com/wolfpld/tracy))
```git 
git submodule --init --recursive
```

## Usage

### Usage Overview

- For **Debug** and **Release** builds, you can toggle Tracy Profiler via `TRACY_ENABLE` CMake option:
```cmake
cmake -B builds -DCMAKE_BUILD_TYPE=Debug -DTRACY_ENABLE=ON
```
Once you set `TRACY_ENABLE=ON` (or `OFF`), that setting persists for subsequent CMake reconfigurations until you override it again. For build performance, when `TRACY_ENABLE` is turned `OFF`, the library isn't added.

- Since a VST3 is a DLL, the Tracy configuration is not the same as for an executable. Thus it is necessary to differentiate two different builds for **Standalone** and for **VST3** formats. This can be done with the `PLUGIN_FORMAT` CMake option:
```cmake
cmake -B builds -DCMAKE_BUILD_TYPE=Debug -DTRACY_ENABLE=ON -DPLUGIN_FORMAT=VST3
```

- To profile your plugin during execution, you can launch the Tracy executable (previously built or directly downloaded in their [Releases page](https://github.com/wolfpld/tracy/releases) for Windows). The connection will occur only when both the *server* (Tracy profiler) and the *client* (your plugin) are running.

### Defining Profiling Targets

- The simplest way to add a function to profile is to add these lines at the beginning of it:
```cpp
#ifdef TRACY_ENABLE
    ZoneScoped;
#endif
```
In fact, all the Tracy commands must be surrounded by the `TRACY_ENABLE` definition condition to ensure that the program runs smoothly even when `TRACY_ENABLE=OFF`.

- All Tracy functions are described in [Tracy documentation](https://github.com/wolfpld/tracy/releases/latest/download/tracy.pdf)

## Documentation

### Useful Resources
- [Official Tracy documentation](https://github.com/wolfpld/tracy/releases/latest/download/tracy.pdf)
- [Tracy repository](https://github.com/wolfpld/tracy?tab=readme-ov-file)
- [JUCE repository](https://github.com/juce-framework/JUCE)
- [JUCE documentation](https://juce.com/learn/documentation/)

### Useful Tools
- [pluginval](https://github.com/Tracktion/pluginval), *"a cross-platform plugin validator and tester application. It is designed to be used by both plugin and host developers to ensure stability and compatibility between plugins and hosts."*

## Project Status

This template was initially developed for a personal project requiring detailed and real time analysis around a plugin developed with the JUCE framework for Windows. I thought it would be useful to share it with other users in the community who have the same profiling and optimization needs.

Any suggestions for approaches, tools or code are welcome, and I'd be delighted to make changes in the future!

Feel free to contact me on the server [Audio Programmer Discord](https://www.theaudioprogrammer.com/discord) or on the JUCE forum.

## License

- Template : [MIT](https://choosealicense.com/licenses/mit/)
- Tracy : [3-clause BSD license](https://opensource.org/license/bsd-3-clause)