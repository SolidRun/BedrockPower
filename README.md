# BedrockPower
Adjust power management settings for SolidRun Bedrock.


Based on: [FlyGoat/ryzen_nb_smu](https://github.com/flygoat/ryzen_nb_smu) & [FlyGoat/ryzenAdj](https://github.com/FlyGoat/RyzenAdj.git)

RyzenAdjUI_WPF by "JustSkill" is no longer maintained, for GUI please see  [Universal x86 Tuning Utility](https://github.com/JamesCJ60/Universal-x86-Tuning-Utility) or [ryzen-controller-team/ryzen-controller](https://gitlab.com/ryzen-controller-team/ryzen-controller/).

## Usage
The command line interface is identical on both Windows and Unix-Like OS.

You should run it with Administrator on Windows or root on Linux.

You can write a shell script or bat to do it automaticly.

```
$./bedrockpower -h
Usage: bedrockpower [options]

 Bedrock Power Management adjust tool.

    -h, --help                            show this help message and exit

Options
    -i, --info                            Show information and most important power metrics after adjustment
    --dump-table                          Show whole power metric table before and after adjustment

Settings
    --power_limit=<u32>       Set system power limit (W) (value between 8 and 54)
    --power-saving                        Hidden options to improve power efficiency (is set when AC unplugged): behavior depends on CPU generation, Device and Manufacture
    --max-performance                     Hidden options to improve performance (is set when AC plugged in): behavior depends on CPU generation, Device and Manufacture
```

### Demo
Example to set the cpu Power Limit to 45W, and Tctl to 90 °C:

    ./bedrockpower --power_limit=45 --tctl-temp=90

### Documentation
- [Supported Models (from original repository)](https://github.com/FlyGoat/RyzenAdj/wiki/Supported-Models)
- [Options](https://github.com/FlyGoat/RyzenAdj/wiki/Options)
- [FAQ (from original repository)](https://github.com/FlyGoat/RyzenAdj/wiki/FAQ)

## Installation

You don't need to install bedrockpower because it does not need configuration, everything is set via arguments
However, some settings could get overwritten by power management features of your device, and you need to regularly set your values again.

We did provide some examples for automation. And these require configuration during installation.

### Windows Installation

Before you start installing anything, it is highly recommended getting familiar with RyzenAdj to find out what can be done on your device.
Use the CLI `ryzenadj.exe` to test the support of your device and to benchmark the effects of each setting.
If your values don't stay persistent you may want to consider installing our example script for automation.

1. Prepare your favorite RyzenAdj arguments
1. Copy the content of your RyzenAdj folder to the final destination
1. Put your configuration into `readjustService.ps1` and test it as administrator until everything works as expected
1. Install `readjustService.ps1` as Task for Windows Task Scheduler by running `installServiceTask.bat`

Deinstallation of the Task can be done via `uninstallServiceTask.bat`

Over Windows Task Scheduler you can check if it is running. It is called `RyzenAdj` below `AMD` folder.
Or just run

    SCHTASKS /query /TN "AMD\RyzenAdj"

### Linux Installation
    Because it is very easy to build the latest version of bedrockpower on Linux, we don't provide precompiled packages for distributions.
    Just follow the build instructions below and you are ready to use it.

## Build

### Build Requirements

Building this tool requires C & C++ compilers as well as **cmake**. It
requires privileged access to NB PCI config space, in order to compile it
one must have pcilib library & headers available.

### Linux
Please make sure that you have libpci dependency before compiling. On
Debian-based distros this is covered by installing **pcilib-dev** package:

    sudo apt install libpci-dev

On Fedora:

    sudo dnf install pciutils-devel

The simplest way to build it:

    git clone https://github.com/FlyGoat/RyzenAdj.git
    cd RyzenAdj
    rm -r win32
    mkdir build && cd build
    cmake -DCMAKE_BUILD_TYPE=Release ..
    make
    if [ -d "~/.local/bin" ]; then ln -s ryzenadj ~/.local/bin/ryzenadj && echo "symlinked to ~/.local/bin/ryzenadj"
    if [ -d "~/.bin" ]; then ln -s ryzenadj ~/.bin/ryzenadj && echo "symlinked to ~/.bin/ryzenadj"

### Windows

It can be built by Visual Studio + MSVC automaticaly, or Clang + Nmake in command line.
However, as for now, MingW-gcc can't be used to compile for some reason.

Required dll is included in ./win32 of source tree. Please put the dll
library and sys driver in the same folder with ryzenadj.exe.

We don't recommend you to build by yourself on Windows since the environment configuarion
is very complicated. If you would like to use ryzenadj functions in your program, see libryzenadj.
