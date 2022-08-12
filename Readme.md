# Dolphin - GameCube和Wii模拟器

[主页](https://dolphin-emu.org/) | [项目网站](https://github.com/dolphin-emu/dolphin) | [构建机器人](https://dolphin.ci) | [论坛](https://forums.dolphin-emu.org/) | [维基](https://wiki.dolphin-emu.org/) | [Issue追踪](https://bugs.dolphin-emu.org/projects/emulator/issues) | [代码风格](https://github.com/dolphin-emu/dolphin/blob/master/Contributing.md) | [Transifex网页](https://www.transifex.com/projects/p/dolphin-emu/)

Dolphin是一个可以在Windows,Linux,Mac OS和近些年来发布的 Android设备上运行GameCube和Wii游戏的模拟器。它是根据条款许可的
GNU通用公共许可证，版本2或更高版本（GPLv2+）。

请在阅读[常见问题回复](https://dolphin-emu.org/docs/faq/)后再使用Dolphin。

## 系统要求

### 桌面端

* OS
    * Windows (10或更高版本).
    * Linux.
    * macOS (10.14 Mojave或更高版本).
    * 除了Linux之外的类UNIX系统不受官方支持但也许能运行。
* 处理器
    * 支持SSE2 的CPU。
    * 推荐近些年来才上市的CPU(双核3GHz，上市时间不得早于2008年)
* 图形
    * 好显卡(支持Direct3D 11.1 / OpenGL 3.3)
    * 推荐支持 Direct3D 11.1 / OpenGL 4.4的显卡

### Android

* OS
    * Android (5.0 Lollipop或更高版本).
* 处理器
    * 支持64位应用程序的处理器(ARMv8或 x86-64)
* 图形
    * 一个支持OpenGL ES 3.0或更高版本的GPU。[设备质量](https://dolphin-emu.org/blog/2013/09/26/dolphin-emulator-and-opengl-drivers-hall-fameshame/)会对模拟性能有很大的影响。
    * 支持桌面端OpenGL功能的GPU可以提供最佳性能。

Dolphin只能安装在满足上述要求的设备上。试图安装在不受支持的设备上将会无法运行且会弹出一条错误信息。

## 为Windows端构建Dolphin

使用解决方案文件`Source/dolphin-emu.sln`来在Windows上构建Dolphin。Visual Studio版本必须得是2022 17.2.3或更高的版本。其他的编译程序也许可以在Windows端上构建Dolphin，但是这种方式未经我们测试且不推荐。在构建之前必须安装Git和Windows 11 SDK。

在构建之前使用以下命令确保子模块已被拉取下来:
```sh
git submodule update --init
```

"Release"解决方案包含最好的性能优化体验但也使调试Dolphin变得更加复杂。

"Debug"解决方案配置过程会比"Release"解决方案更加漫长，更加详细和更加宽松，但这也使调试Dolphin变得更加简单。

在Installer目录中的`Installer.nsi`会创建一个安装器Installer。这需要安装Nullsoft Scriptable Install System
(NSIS) 自从二进制文件夹包含整个Dolphin发行版所需要的文件时，就不再需要创建一个安装器Installer来运行Dolphin了

## 为Linux和Mac OS构建Dolphin

除Windows系统以外，Dolphin都需要CMake来构建。许多的库都是与Dolphin捆绑在一起的；如果这些东西你没有装在系统上，那你就可以使用它们。如果一个内置的库需要被用到，而你安装的软件包还不足够完整时CMake就会通知你并让你安装缺失的软件包。你可能需要参考一下[维基界面](https://github.com/dolphin-emu/dolphin/wiki/Building-for-Linux)来了解更多。

在构建之前使用以下命令确保子模块已被拉取下来:
```sh
git submodule update --init
```

### macOS构建步骤:

你可以参考以下步骤来构建一个支持单一架构的二进制文件: 

1. `mkdir build`
2. `cd build`
3. `cmake ..`
4. `make`

一个软件包将会在`./Binaries`中创建。

我们提供了一个可以构建支持x64和ARM的通用应用程序，如需构建，请参考以下步骤：

1. `mkdir build`
2. `cd build`
3. `python ../BuildMacOSUniversalBinary.py`
4. 通用二进制文件将会在`universal`文件夹内。

如果你要这么做，那么构建将会变得更加复杂，因为它需要适用于x64和ARM架构的安装库依赖项（或者通用库）而且你还需要指定附加参数来指向相关库的路径。请执行 `BuildMacOSUniversalBinary.py --help`以获取更多帮助。

### Linux通用构建指南:

安装到你的系统中

1. `mkdir build`
2. `cd build`
3. `cmake ..`
4. `make`
5. `sudo make install`

### Linux本地构建指南:

适用于无root权限的构建

1. `mkdir Build`
2. `cd Build`
3. `cmake .. -DLINUX_LOCAL_DEV=true`
4. `make`
5. `ln -s ../../Data/Sys Binaries/`

### Linux便携版构建指南:

可以被储存在各式各样的外置存储设备上而且还可以在不同的Linux系统上使用。
还可用于对不同的Dolphin进行调试/开发/TAS。

1. `mkdir Build`
2. `cd Build`
3. `cmake .. -DLINUX_LOCAL_DEV=true`
4. `make`
5. `cp -r ../Data/Sys/ Binaries/`
6. `touch Binaries/portable.txt`

## 为Android构建

阅读以下说明，你需要熟悉Android的开发。如果你还没有准备好Android开发环境，请详阅[AndroidSetup.md](AndroidSetup.md) 。

在构建之前使用以下命令确保子模块已被拉取下来:
```sh
git submodule update --init
```

如果你用Android Studio来进行调试，请导入位于`./Source/Android`的Gradle项目。

Android应用是通过一个叫做Gradle的构建系统来编译的。然而，Dolphin的原生组件是使用CMake来编译的。 在编译Java代码时，Gradle会试图去运行一个CMake构建。

## 卸载

When Dolphin has been installed with the NSIS installer, you can uninstall
Dolphin like any other Windows application.

Linux users can run `cat install_manifest.txt | xargs -d '\n' rm` as root from the build directory
to uninstall Dolphin from their system.

macOS users can simply delete Dolphin.app to uninstall it.

Additionally, you'll want to remove the global user directory (see below to
see where it's stored) if you don't plan to reinstall Dolphin.

## Command Line Usage

`Usage: Dolphin [-h] [-d] [-l] [-e <str>] [-b] [-v <str>] [-a <str>]`

* -h, --help Show this help message
* -d, --debugger Show the debugger pane and additional View menu options
* -l, --logger Open the logger
* -e, --exec=<str> Load the specified file (DOL,ELF,WAD,GCM,ISO)
* -b, --batch Exit Dolphin with emulator
* -v, --video_backend=<str> Specify a video backend
* -a, --audio_emulation=<str> Low level (LLE) or high level (HLE) audio

Available DSP emulation engines are HLE (High Level Emulation) and
LLE (Low Level Emulation). HLE is faster but less accurate whereas
LLE is slower but close to perfect. Note that LLE has two submodes (Interpreter and Recompiler)
but they cannot be selected from the command line.

Available video backends are "D3D" and "D3D12" (they are only available on Windows), "OGL", and "Vulkan".
There's also "Null", which will not render anything, and
"Software Renderer", which uses the CPU for rendering and
is intended for debugging purposes only.

## Sys Files

* `wiitdb.txt`: Wii title database from [GameTDB](https://www.gametdb.com/)
* `totaldb.dsy`: Database of symbols (for devs only)
* `GC/font_western.bin`: font dumps
* `GC/font_japanese.bin`: font dumps
* `GC/dsp_coef.bin`: DSP dumps
* `GC/dsp_rom.bin`: DSP dumps
* `Wii/clientca.pem`: Wii network certificate
* `Wii/clientcakey.pem`: Wii network certificate key
* `Wii/rootca.pem`: Wii network certificate issuer / CA

The DSP dumps included with Dolphin have been written from scratch and do not
contain any copyrighted material. They should work for most purposes, however
some games implement copy protection by checksumming the dumps. You will need
to dump the DSP files from a console and replace the default dumps if you want
to fix those issues.

Wii network certificates must be extracted from a Wii IOS. A guide for that can be found [here](https://wiki.dolphin-emu.org/index.php?title=Wii_Network_Guide).

## Folder Structure

These folders are installed read-only and should not be changed:

* `GameSettings`: per-game default settings database
* `GC`: DSP and font dumps
* `Shaders`: post-processing shaders
* `Themes`: icon themes for GUI
* `Resources`: icons that are theme-agnostic
* `Wii`: default Wii NAND contents

## Packaging and udev

The Data folder contains a udev rule file for the official GameCube controller
adapter and the Mayflash DolphinBar. Package maintainers can use that file in their packages for Dolphin.
Users compiling Dolphin on Linux can also just copy the file to their udev
rules folder.

## User Folder Structure

A number of user writeable directories are created for caching purposes or for
allowing the user to edit their contents. On macOS and Linux these folders are
stored in `~/Library/Application Support/Dolphin/` and `~/.dolphin-emu`
respectively, but can be overwritten by setting the environment variable
`DOLPHIN_EMU_USERPATH`. On Windows the user directory is stored in the `My Documents`
folder by default, but there are various way to override this behavior:

* Creating a file called `portable.txt` next to the Dolphin executable will
  store the user directory in a local directory called "User" next to the
  Dolphin executable.
* If the registry string value `LocalUserConfig` exists in
  `HKEY_CURRENT_USER/Software/Dolphin Emulator` and has the value **1**,
  Dolphin will always start in portable mode.
* If the registry string value `UserConfigPath` exists in
  `HKEY_CURRENT_USER/Software/Dolphin Emulator`, the user folders will be
  stored in the directory given by that string. The other two methods will be
  prioritized over this setting.

List of user folders:

* `Cache`: used to cache the ISO list
* `Config`: configuration files
* `Dump`: anything dumped from Dolphin
* `GameConfig`: additional settings to be applied per-game
* `GC`: memory cards and system BIOS
* `Load`: custom textures
* `Logs`: logs, if enabled
* `ScreenShots`: screenshots taken via Dolphin
* `StateSaves`: save states
* `Wii`: Wii NAND contents

## Custom Textures

Custom textures have to be placed in the user directory under
`Load/Textures/[GameID]/`. You can find the Game ID by right-clicking a game
in the ISO list and selecting "ISO Properties".
