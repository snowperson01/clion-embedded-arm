Purpose
===

The plugin supports two different, almost unrelated features:
 * Conversion STM32CubeMX projects to CLion-capable _cmake_ project
 * Downloading and debugging binaries onto MCU chips using [OpenOCD](http://openocd.org/)

You can use only one the second parts, if you develop something unrelated to ST products.

Disclaimer
===
You are doing everything at your own risk. Nor me, nor JetBrains, nor anybody else takes any 
responsibility in case of any direct or indirect damages or losses. 

Prerequisites
===
You will need following tools being installed and configured:

 * Compatible hardware, virtually any of [STM32 development boards](http://www.st.com/en/evaluation-tools/stm32-mcu-eval-tools.html)
 * [CLion](https://www.jetbrains.com/clion/). The project tested against CLion 2017.3.
 * [GNU Arm Embedded Toolchain](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm) 
 * [OpenOCD](http://openocd.org/)
 * **(Windows only)** ST-LINK/V2 driver. May be downloaded from 
 [st.com](http://www.st.com/en/development-tools/stsw-link009.html) or just borrowed from OpenOCD binary distribution
 * [STM32CubeMX](http://www.st.com/en/development-tools/stm32cubemx.html). After installation, do not forget
 to download MCU support library for your MCU. See _Help -> Install new libraries_ there.
 * **(Windows only)** [MinGW](http://www.mingw.org/)

Install Plugin 
===
In CLion, go to **File  ->  Settings ... ->  Plugins  ->  Browse repositories ...** and install the plugin **"OpenOCD + STM32CubeMX support for ARM embedded"**.

Project creation and conversion HowTo
===
 1. Run _STM32CubeMX_ and:
    1. Choose your hardware
    1. Configure it
    1. In project settings, select name and location for the project.
    1. In project settings, select **SW4STM32** as a toolchain.
    1. Click **Generate Code**. This will generate Eclipse-style project stub with libraries and sources. 
 1. Run _Clion_ and:
    1. Open or import the result folder of the previous step as a project. Ignore all the errors shown.
    1. Go to **File -\> Settings...  -\> Build, Execution, Deployment -\> OpenOCD support** and configure tools locations and tcp ports.
    1. In the same dialog, select your board config file. OpenOCD is shipped with a set of board config files located at 
    */usr/share/openocd/scripts/board* folder, in case of Windows *\<openocd_home\>/share/openocd/scripts/board*. Those files are 
    _OpenOCD_ predefined ones and they are quite obviously named, for instance *st_nucleo_f4.cfg* is the config file for any *STM32 Nucleo* boards based on
      STM32F4 MCU family. If there is no suitable config among existing, you can write your own *.cfg* file and use it. 
      Refer to OpenOCD documentation for more details. 
    1. Select **Tools -\> Update CMake project with STM32CubeMX project**. This will regenerate project files and reload _cmake_ configs.

Now you can connect your board, compile and start the firmware. The plugin creates special run configuration, 
if you run it, the compiled firmware will be downloaded to the target board, and then the chip will be reset. 
If **Debug** button is pressed, then firmware will be downloaded, chip chip will be reset, and then remote debugger will be attached 
to the MCU, you can use breakpoints and watches to verify how your firmware works on-chip.
 
    
While code writing    
===
 * Put all your source and include files under _Src_ and _Inc_ folders, respectively. 
 * In files, generated by **STM32CubeMX** all your code lines should be placed between of 
`/* USER CODE BEGIN ??? */` and `/* USER CODE END ??? */` pseudo comments. **STM32CubeMX** keeps those pieces of 
code untouched during code regeneration.
 * Run **Update CMake project with STM32CubeMX project** everytime after running **STM32CubeMX** code regeneration.
 
 * _CMakeLists.txt_ is always regenerated during project update, so if you make changes to it, they all will be lost. 
 If you need to have _CMakeLists.txt_ changed (i.e. external libraries, FPU support etc.) please 
 change the template, _CMakeLists_template.txt_, and then update the project.
 
Bugs, Feature requests, Questions, and Contribution
===

Please read [CONTRIBUTING.md](CONTRIBUTING.md)

Likes and Donations
===

If you like the plugin, you may :star: the project at github (button at top-right of the page) and at [jetbrains plugins repository](https://plugins.jetbrains.com/plugin/10115).

The plugin is free, but you can support my work with a donation. 

[10 EUR](https://paypal.me/elmot/10eur) |
[5 EUR](https://paypal.me/elmot/5eur) |
[Other amount](https://paypal.me/elmot)

![PayPal](https://www.paypalobjects.com/webstatic/mktg/Logo/pp-logo-100px.png) 
