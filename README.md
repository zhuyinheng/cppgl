CppGL Framework Mac
## Changes for macOS Compatibility

To ensure compatibility with macOS, the following adjustments were made:

1. Removed `std++fs` in CMake.
2. Changed OpenGL version from 4.30 to 4.10 due to macOS limitations.
3. Disabled compute shader support because of the lower OpenGL version.

```
CppGL Framework
=======

An OpenGL framework designed for teaching and research, emphasizing simplicity and reusability.

## Dependencies

### Ubuntu (22.04)

    apt-get install -y build-essential libwayland-dev libxkbcommon-dev xorg-dev libopengl-dev freeglut3-dev cmake

### Windows

    Visual Studio build tools 2019 or newer and editor with CMake support

## Clone

Use:

    git clone --recursive git@git9.cs.fau.de:renderingframeworks/cppgl.git


Or, after clone use:

    git submodule update --init

## Build

Without examples:

    cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -Wno-dev && cmake --build build --parallel

With examples:

    rm -rf build && cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DCPPGL_BUILD_EXAMPLES=ON -Wno-dev -G Ninja && cmake --build build --parallel

## Examples

Included is an example rendering application loading a ```.obj``` file from the command line and rendering it with a standard diffuse shader.
If no object file is given, a rgb debug color is displayed.
Also included is a compute shader example, which changes the output color to a greyscale image.
The debug menu can be accessed by pressing ```F1```.

## Issues / Suggestions / Feedback

Please mail to <nikolai.hofmann@fau.de>, <laura.fink@fau.de> or <linus.franke@fau.de>.

```