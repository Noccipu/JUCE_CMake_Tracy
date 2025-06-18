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