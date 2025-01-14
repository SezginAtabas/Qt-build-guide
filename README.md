# Qt-build-guide
This repository explains how to build Qt from source to activate certain features such as WebRTC support within QtWebEngine.
This guide is mainly for linux platforms and has been tested on Ubuntu 22.04.


## Workflow
There are 3 steps to building Qt from source, configure, build, install.
In the configuration step we will choose which modules we will build Qt with. This is where we decide on what features we want to include in the build.
After the configuration we will go on to build Qt using CMake after which we will install the built Qt.

## Dependencies
The dependencies that you require for the build will differ based on the chosen modules, below are instructions on how to setup for a some of them.

### XCB - X11 support
```bash
sudo apt-get install libx11-dev libx11-dev libxkbfile-dev libsecret-1-dev libxkbcommon-x11-dev libxshmfence-dev libxc*
```

## QWebEngine Dependencies

[Qt WebEngine Platform Notes](https://doc.qt.io/qt-6/qtwebengine-platform-notes.html)

On all platforms, the following tools are required at build time:

    C++20 compiler support
    CMake 3.19 or newer
    Python 3 with html5lib library
    Bison, Flex
    GPerf
    Node.js version 14 or later

For Node.js configuration we need to make sure that the node version used is version 14 or later.
The old nodejs packages installed via apt interfere with and cause the old node version to be called
which is located at `/usr/bin/nodejs`. To remedy this remove old node packages that might be installed via apt.

```bash
sudo apt-get remove nodejs
sudo apt-get purge nodejs
sudo apt-get autoremove
```

To install Node.js use [nvm](https://github.com/nvm-sh/nvm).

```bash
sudo apt-get install bison flex gperf
```

```bash
sudo apt-get install bison build-essential gperf flex python2 libasound2-dev \
libcups2-dev libdrm-dev libegl1-mesa-dev libnss3-dev libpci-dev libpulse-dev libudev-dev nodejs \
libxtst-dev gyp ninja-build
```

```bash
sudo apt-get install libssl-dev libxcursor-dev libxcomposite-dev libxdamage-dev libxrandr-dev \
libfontconfig1-dev libxss-dev libwebp-dev libjsoncpp-dev libopus-dev libminizip-dev \
libavutil-dev libavformat-dev libavcodec-dev libevent-dev libvpx-dev libsnappy-dev libre2-dev libprotobuf-dev protobuf-compiler
```

## Configure
For this step we will use the configuration tool provided by QT. You can find the configure tool at your Qt install source directory.
For example by default it will be at the `~/Qt` directory. see [Qt Configure Options](https://doc.qt.io/qt-6/configure-options.html) for more details.

For all the configuration options use `-h` flag.
```bash
~/Qt/6.8.1/Src/configure -h
```

Create a new directory to store custom Qt builds.
```bash
mkdir -p ~/custom_qt_builds/my_build
cd ~/custom_qt_builds/my_build
```

Choose which modules you want to include in your build, then run the configure command.
Example Configure command:
```bash
~/Qt/6.8.1/Src/configure -webengine-proprietary-codecs -webengine-webrtc -xcb --xcb-xlib -cmake-generator Ninja -- -Wno-dev
```

NOTE: It is recommended to use Ninja as the CMake generator. All other generators are not officially supported.

## Build And Install

Build
```bash
cmake --build . --parallel
```

Install
```bash
sudo cmake --install .
```
By default the installation directory is `/usr/local/Qt-6.8.1` and can be changed with `-prefix` option at the configuration step. See [here](https://doc.qt.io/qt-6/configure-options.html#source-build-and-install-directories).


