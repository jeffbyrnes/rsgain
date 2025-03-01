# BUILDING
rsgain builds natively on Unix and Windows, and features a cross-platform CMake build system. The following external dependencies are required:
- libebur128
- taglib
- FFmpeg, specifically the libraries:
	+ libavformat
	+ libavcodec
	+ libswresample
	+ libavutil
- fmt
- inih

The source code is written in C++20, and as such requires a relatively modern compiler to build:
- On Windows, use Visual Studio 2022
- On Linux, use GCC 10 or later
- On macOS, the latest available Xcode for your machine should work

## Unix
Before starting, make sure you have the development tools Git, CMake and pkg-config installed.

Install the required dependencies:

### APT-based Linux (Debian, Ubuntu, Mint etc.)
```
sudo apt install libebur128-dev libtag1-dev libavformat-dev libavcodec-dev libswresample-dev libavutil-dev libfmt-dev libinih-dev
```
### Pacman-based Linux (Arch, Manjaro, etc.)
```
sudo pacman -S libebur128 taglib ffmpeg fmt libinih
```
### DNF-based Linux (Fedora)
FFmpeg is in the official repos in Fedora 36 and later only. If you're on an earlier version you will need to find an alternative source.
```
sudo dnf install libebur128-devel taglib-devel libavformat-free-devel libavcodec-free-devel libswresample-free-devel libavutil-free-devel fmt-devel inih-devel
```
### macOS (Homebrew)
```
brew install libebur128 taglib ffmpeg fmt inih 
```

### FreeBSD (Packages)
```
pkg install libebur128 taglib libfmt inih ffmpeg
```

### Building
Clone the master repo and create a build directory:
```
git clone https://github.com/complexlogic/rsgain.git
cd rsgain
mkdir build && cd build
```
Generate the Makefile:
```
cmake .. -DCMAKE_BUILD_TYPE=Release
```
Build and test the program:
```
make
./rsgain
```
Optionally, install to your system directories:
```
sudo make install
```
By default, this will install rsgain with a prefix of `/usr/local`. If you want a different prefix, re-run the CMake generation step with `-DCMAKE_INSTALL_PREFIX=prefix`.

#### Deb Packages
The build system includes support for .deb packages via CPack. Pass `-DPACKAGE=DEB` and `-DCMAKE_INSTALL_PREFIX=/usr` to cmake. Then, build the package with:
```
make package
```
By default, this will build a package for the amd64 architechture. To build the package for a different architecture, pass `-DCPACK_DEBIAN_PACKAGE_ARCHITECTURE=architecture` to cmake.

## Windows
The Windows toolchain consists of Visual Studio and vcpkg in addition to Git and CMake. Before starting, make sure that Visual Studio is installed with C++ core desktop features and C++ CMake tools. The free Community Edition is sufficient.

Clone the master repo and create a build directory:
```
git clone https://github.com/complexlogic/rsgain.git
cd rsgain
mkdir build
cd build
```
Build the dependencies and generate the Visual Studio project files:
```
git clone https://github.com/microsoft/vcpkg
cmake .. -DCMAKE_TOOLCHAIN_FILE=".\vcpkg\scripts\buildsystems\vcpkg.cmake" -DVCPKG_TARGET_TRIPLET="x64-windows"
```
Build and test the program:
```
cmake --build .
.\Debug\rsgain.exe
```
Optionally, generate a zipped install package:
```
cmake --build . --config Release --target package
```
