# BedrockPower
Adjust power management settings for SolidRun Bedrock.


Based on: [FlyGoat/ryzen_nb_smu](https://github.com/flygoat/ryzen_nb_smu) & [FlyGoat/ryzenAdj](https://github.com/FlyGoat/RyzenAdj.git)

## Usage
The command line interface is identical on both Windows and Unix-Like OS.

You should run it with Administrator on Windows or root on Linux.

You can write a shell script or bat to do it automaticly.

```
$ ./bedrockpower -h
Usage: ryzenadj [options]

 Ryzen Power Management adjust tool.

    -h, --help                show this help message and exit

Options
    -i, --info                Show information and most important power metrics after adjustment
    --dump-table              Show whole power metric table before and after adjustment

Settings
    -t, --tctl-temp=<u32>     Tctl Temperature Limit (degree C)
    -s, --power-saving        Hidden options to improve power efficiency (is set when AC unplugged): behavior depends on CPU generation, Device and Manufacture
    -p, --max-performance     Hidden options to improve performance (is set when AC plugged in): behavior depends on CPU generation, Device and Manufacture
    -l, --power_limit=<u32>   Set system power limit (W) (value between 8 and 54)
```

### Demo
Example to set the cpu Power Limit to 45W, and Tctl to 90 °C:

    ./bedrockpower --power_limit=45 --tctl-temp=90

### Documentation
- [Supported Models (from original repository)](https://github.com/FlyGoat/RyzenAdj/wiki/Supported-Models)
- [Options](https://github.com/FlyGoat/RyzenAdj/wiki/Options)
- [FAQ (from original repository)](https://github.com/FlyGoat/RyzenAdj/wiki/FAQ)
## Build

### Build Requirements

Building this tool requires C & C++ compilers as well as **cmake**.\
It requires privileged access to NB PCI config space, in order to compile it
one must have pcilib library & headers available.

### Linux
Please make sure that you have libpci dependency before compiling. On
Debian-based distros this is covered by installing **pcilib-dev** package:

    sudo apt install libpci-dev

On Fedora:

    sudo dnf install pciutils-devel

The simplest way to build it:

    git clone https://github.com/SolidRun/BedrockPower.git
    cd BedrockPower
    rm -r win32
    mkdir build && cd build
    cmake -DCMAKE_BUILD_TYPE=Release ..
    make
    if [ -d "~/.local/bin" ]; then ln -s bedrockpower ~/.local/bin/bedrockpower && echo "symlinked to ~/.local/bin/bedrockpower"
    if [ -d "~/.bin" ]; then ln -s bedrockpower ~/.bin/bedrockpower && echo "symlinked to ~/.bin/bedrockpower"

### Windows
We recommend to use the [Universal x86 Tuning Utility](https://github.com/JamesCJ60/Universal-x86-Tuning-Utility).

