# 编译 PX4 软件

对于模拟目标和硬件目标，可以在控制台上或 IDE 中生成 PX4。

PX4 源代码存储在 [PX4/Firmware](https://github.com/PX4/Firmware) 存储库中的 GitHub 上。 若要在您的计算机上获得*最新的*版本，请在终端中输入以下命令：

初次编译之前，我们会使用终端环境编译一个模拟目标。 这使我们能够在进入真正的硬件和 IDE 之前验证系统设置。

## 下载 PX4 源代码

The PX4 source code is stored on Github in the [PX4/PX4-Autopilot](https://github.com/PX4/PX4-Autopilot) repository.

To get the *very latest* ("main") version onto your computer, enter the following command into a terminal:

```sh
git clone --recursive https://github.com/google/bloaty.git /tmp/bloaty \
      && cd /tmp/bloaty && cmake -GNinja . && ninja bloaty && cp bloaty /usr/local/bin/ \
      && rm -rf /tmp/*
```

:::note
This is all you need to do just to build the latest code. If needed you can also [get the source code specific to a particular release](../contribute/git_examples.md#get-a-specific-release). [GIT Examples](../contribute/git_examples.md) provides a lot more information working with releases and contributing to PX4.
:::

## 初次编译（使用 jMAVSim 模拟器）

首先我们要用控制台（小黑窗）来构建一个模拟模拟目标 This allows us to validate the system setup before moving on to real hardware and an IDE.

导航到 **Firmware** 目录，并使用以下命令启动 [jMAVSim](../simulation/jmavsim.md)：
```sh
make px4_sitl jmavsim
```

This will bring up the PX4 console below:

![PX4 Console (jMAVSim)](../../assets/toolchain/console_jmavsim.png)

:::note
You may need to start *QGroundControl* before proceeding, as the default PX4 configuration requires a ground control connection before takeoff. This can be [downloaded from here](https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html).
:::

The drone can be flown by typing:
```sh
pxh> commander takeoff
```

![jMAVSim UI](../../assets/toolchain/jmavsim_first_takeoff.png)

The drone can be landed by typing `commander land` and the whole simulation can be stopped by doing **CTRL+C** (or by entering `shutdown`).

Flying the simulation with the ground control station is closer to the real operation of the vehicle. Click on a location in the map while the vehicle is flying (takeoff flight mode) and enable the slider. This will reposition the vehicle.

![QGroundControl GoTo](../../assets/toolchain/qgc_goto.jpg)

:::tip PX4 can be used with a number of other [Simulators](../simulation/README.md), including [Gazebo Simulation](../simulation/gazebo.md) and [AirSim Simulation](../simulation/airsim.md). These are also started with *make* - e.g.
```
-- Build files have been written to: /home/youruser/src/Firmware/build/px4_fmu-v4_default
[954/954] Creating /home/youruser/src/Firmware/build/px4_fmu-v4_default/px4_fmu-v4_default.px4
```
运行成功后将输出类似结束：

## 基于NuttX / Pixhawk 的飞控板

### 获取特定发行版本

下面的列表是常见飞控板的生成命令：

For example, to build for [Pixhawk 4](../flight_controller/pixhawk4.md) hardware you could use the following command:
```sh
cd PX4-Autopilot
make px4_fmu-v5_default
```

A successful run will end with similar output to:
```sh
-- Build files have been written to: /home/youruser/src/PX4-Autopilot/build/px4_fmu-v4_default
[954/954] Creating /home/youruser/src/PX4-Autopilot/build/px4_fmu-v4_default/px4_fmu-v4_default.px4
```

The first part of the build target `px4_fmu-v4` indicates the firmware for a particular flight controller hardware. The following list shows the build commands for the [Pixhawk standard](../flight_controller/autopilot_pixhawk_standard.md) boards:

- [Holybro Pixhawk 6X (FMUv6X)](../flight_controller/pixhawk6x.md): `make px4_fmu-v6x_default`
- [Holybro Pixhawk 6C (FMUv6C)](../flight_controller/pixhawk6c.md): `make px4_fmu-v6c_default`
- [Holybro Pix32 v6 (FMUv6C)](../flight_controller/holybro_pix32_v6.md): `make px4_fmu-v6c_default`
- [Holybro Pixhawk 5X (FMUv5X)](../flight_controller/pixhawk5x.md): `make px4_fmu-v5x_default`
- [Pixhawk 4 (FMUv5)](../flight_controller/pixhawk4.md): `make px4_fmu-v5_default`
- [Pixhawk 4 Mini (FMUv5)](../flight_controller/pixhawk4_mini.md): `make px4_fmu-v5_default`
- [CUAV V5+ (FMUv5)](../flight_controller/cuav_v5_plus.md): `make px4_fmu-v5_default`
- [CUAV V5 nano (FMUv5)](../flight_controller/cuav_v5_nano.md): `make px4_fmu-v5_default`
- [Pixracer (FMUv4)](../flight_controller/pixracer.md): `make px4_fmu-v4_default`
- [Pixhawk 3 Pro](../flight_controller/pixhawk3_pro.md): `make px4_fmu-v4pro_default`
- [Pixhawk Mini](../flight_controller/pixhawk_mini.md): `make px4_fmu-v3_default`
- [Pixhawk 2 (Cube Black) (FMUv3)](../flight_controller/pixhawk-2.md): `make px4_fmu-v3_default`
- [mRo Pixhawk (FMUv3)](../flight_controller/mro_pixhawk.md): `make px4_fmu-v3_default` (supports 2MB Flash)
- [Holybro pix32 (FMUv2)](../flight_controller/holybro_pix32.md): `make px4_fmu-v2_default`
- [Pixfalcon (FMUv2)](../flight_controller/pixfalcon.md): `make px4_fmu-v2_default`
- [Dropix (FMUv2)](../flight_controller/dropix.md): `make px4_fmu-v2_default`
- [Pixhawk 1 (FMUv2)](../flight_controller/pixhawk.md): `make px4_fmu-v2_default` :::warning You **must** use a supported version of GCC to build this board (e.g. the same as used by [CI/docker](../test_and_ci/docker.md)) or remove modules from the build. Building with an unsupported GCC may fail, as PX4 is close to the board's 1MB flash limit. 若要在您的计算机上获得*最新的*版本，请在终端中输入以下命令：
- Pixhawk 1 with 2 MB flash: `make px4_fmu-v3_default`

Build commands for non-Pixhawk NuttX fight controllers (and for all other-boards) are provided in the documentation for the individual [flight controller boards](../flight_controller/README.md).

:::note
The `_default` suffix is the firmware _configuration_. This is optional (i.e. you can also build using `make px4_fmu-v4`, `make bitcraze_crazyflie`, etc.).
:::

### 将固件烧录到飞控板

Append `upload` to the make commands to upload the compiled binary to the autopilot hardware via USB. For example

```sh
make px4_fmu-v4_default upload
```

并上传：

```sh
Erase  : [====================] 100.0%
Program: [====================] 100.0%
Verify : [====================] 100.0%
Rebooting.

[100%] Built target upload
```

## 其他飞控板

Build commands for other boards are given the [board-specific flight controller pages](../flight_controller/README.md) (usually under a heading *Building Firmware*).

You can also list all configuration targets using the command:
```sh
make list_config_targets
```


## 用图形界面 IDE 编译

[VSCode](../dev_setup/vscode.md) is the officially supported (and recommended) IDE for PX4 development. It is easy to set up and can be used to compile PX4 for both simulation and hardware environments.


## Qt Creator 功能

### 将固件烧录到飞控板

Many build problems are caused by either mismatching submodules or an incompletely cleaned-up build environment. Updating the submodules and doing a `distclean` can fix these kinds of errors:
```
sudo ./bin/px4 -s px4.config
```

### OcPoC-Zynq Mini

The `region 'flash' overflowed by XXXX bytes` error indicates that the firmware is too large for the target hardware platform. This is common for `make px4_fmu-v2_default` builds, where the flash size is limited to 1MB.

If you're building the *vanilla* master branch, the most likely cause is using an unsupported version of GCC. In this case, install the version specified in the [Developer Toolchain](../dev_setup/dev_env.md) instructions.

If building your own branch, it is possibly you have increased the firmware size over the 1MB limit. In this case you will need to remove any drivers/modules that you don't need from the build.


### Parrot Bebop

MacOS allows a default maximum of 256 open files in all running processes. The PX4 build system opens a large number of files, so you may exceed this number.

The build toolchain will then report `Too many open files` for many files, as shown below:
```sh
cd Firmware
make emlid_navio2_native # for native build
```

The solution is to increase the maximum allowed number of open files (e.g. to 300). You can do this in the macOS *Terminal* for each session:
- Run this script [Tools/mac_set_ulimit.sh](https://github.com/PX4/PX4-Autopilot/blob/main/Tools/mac_set_ulimit.sh), or
- Enter this command:
  ```sh
  ulimit -S -n 300
  ```

### Qt creator 提供符号跳转、自动补全和编译固件的功能。

As of macOS Catalina 10.15.1 there may be problems when trying to build the simulator with *cmake*. If you have build problems on this platform then try run the following command in your terminal:
```sh
xcode-select --install
sudo ln -s /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/* /usr/local/include/
```

### 基于 QuRT / Snapdragon 的飞控板

Build issues related to `arm_none_eabi_gcc`may be due to a broken g++ toolchain installation. You can verify that this is the case by checking for missing dependencies using:
```bash
<br />______  __   __    ___
| ___ \ \ \ / /   /   |
| |_/ /  \ V /   / /| |
|  __/   /   \  / /_| |
| |     / /^\ \ \___  |
\_|     \/   \/     |_/

px4 starting.


pxh&gt;
```

Example of bash output with missing dependencies:
```bash
arm-none-eabi-gdb --version
arm-none-eabi-gdb: command not found
```

This can be resolved by removing and [reinstalling the compiler](https://askubuntu.com/questions/1243252/how-to-install-arm-none-eabi-gdb-on-ubuntu-20-04-lts-focal-fossa).

### Ubuntu 18.04: Visual Studio Code is unable to watch for file changes in this large workspace

See [Visual Studio Code IDE (VSCode) > Troubleshooting](../dev_setup/vscode.md#troubleshooting).

### Failed to import Python packages

"Failed to import" errors when running the `make px4_sitl jmavsim` command indicates that some Python packages are not installed (where expected).
```
Failed to import jinja2: No module named 'jinja2'
You may need to install it using:
    pip3 install --user jinja2
```
If you have already installed these dependencies this may be because there is more than one Python version on the computer (e.g. Python 2.7.16 Python 3.8.3), and the module is not present in the version used by the build toolchain.

You should be able to fix this by explicitly installing the dependencies as shown:
```
pip3 install --user pyserial empty toml numpy pandas jinja2 pyyaml pyros-genmsg packaging
```


## PX4 创建生成目标

The previous sections showed how you can call *make* to build a number of different targets, start simulators, use IDEs etc. This section shows how *make* options are constructed and how to find the available choices.

The full syntax to call *make* with a particular configuration and initialization file is:
```sh
make [VENDOR_][MODEL][_VARIANT] [VIEWER_MODEL_DEBUGGER_WORLD]
```

**VENDOR_MODEL_VARIANT**: (also known as `CONFIGURATION_TARGET`)

- **VENDOR:** The manufacturer of the board: `px4`, `aerotenna`, `airmind`, `atlflight`, `auav`, `beaglebone`, `intel`, `nxp`, etc. The vendor name for Pixhawk series boards is `px4`.
- **MODEL:** The *board model* "model": `sitl`, `fmu-v2`, `fmu-v3`, `fmu-v4`, `fmu-v5`, `navio2`, etc.
- **VARIANT:** Indicates particular configurations: e.g. `rtps`, `lpe`, which contain components that are not present in the `default` configuration. Most commonly this is `default`, and may be omitted.

:::tip
You can get a list of *all* available `CONFIGURATION_TARGET` options using the command below:
```sh
make list_config_targets
```
:::

**VIEWER_MODEL_DEBUGGER_WORLD:**

- **VIEWER:** This is the simulator ("viewer") to launch and connect: `gazebo`, `jmavsim`, `none` <!-- , ?airsim -->

:::tip
`none` can be used if you want to launch PX4 and wait for a simulator (jmavsim, gazebo, or some other simulator). For example, `make px4_sitl none_iris` launches PX4 without a simulator (but with the iris airframe). 若要在您的计算机上获得*最新的*版本，请在终端中输入以下命令：
- **MODEL:**要使用的 *载具* 模型（例如 `iris` (*default*)、`rover`、`tailsitter` 等），该模型将由模拟器加载。 环境变量 `PX4_SIM_MODEL` 将设置为所选模型。 然后在 [启动脚本 ](#scripts) 中使用该模型来选择适当的参数。
- **DEBUGGER:**要使用的调试器：`none` (*default*)、`ide`、`gdb`、`lldb`、`ddd`、`valgrind`、`callgrind`。 有关详细信息，请参阅 < 0>Simulation 调试 </0>。
- **WORLD:** (Gazebo only). Set the world ([PX4-Autopilot/Tools/simulation/gazebo/sitl_gazebo/worlds](https://github.com/PX4/PX4-SITL_gazebo/tree/master/worlds)) that is loaded. Default is [empty.world](https://github.com/PX4/PX4-SITL_gazebo/blob/master/worlds/empty.world). For more information see [Gazebo > Loading a Specific World](../simulation/gazebo.md#set_world).

:::tip
You can get a list of *all* available `VIEWER_MODEL_DEBUGGER_WORLD` options using the command below:

```sh
make px4_sitl list_vmd_make_targets
```
:::

Notes:
- Most of the values in the `CONFIGURATION_TARGET` and `VIEWER_MODEL_DEBUGGER` have defaults, and are hence optional. For example, `gazebo` is equivalent to `gazebo_iris` or `gazebo_iris_none`.
- You can use three underscores if you want to specify a default value between two other settings. For example, `gazebo___gdb` is equivalent to `gazebo_iris_gdb`.
- You can use a `none` value for `VIEWER_MODEL_DEBUGGER` to start PX4 and wait for a simulator. For example start PX4 using `make px4_sitl_default none` and jMAVSim using `./Tools/simulation/jmavsim/jmavsim_run.sh -l`.


The `VENDOR_MODEL_VARIANT` options map to particular *px4board* configuration files in the PX4 source tree under the [/boards](https://github.com/PX4/PX4-Autopilot/tree/main/boards) directory. Specifically `VENDOR_MODEL_VARIANT` maps to a configuration file **boards/VENDOR/MODEL/VARIANT.px4board** (e.g. `px4_fmu-v5_default` corresponds to [boards/px4/fmu-v5/default.px4board](https://github.com/PX4/PX4-Autopilot/blob/main/boards/px4/fmu-v5/default.px4board)).

Additional make targets are discussed in relevant sections:
- `bloaty_compare_master`: [Binary Size Profiling]()
- ...


## 列出所有发行版本（标签） sh git tag -l

The *PX4 Firmware Version* and *Custom Firmware Version* are published using the MAVLink [AUTOPILOT_VERSION](https://mavlink.io/en/messages/common.html#AUTOPILOT_VERSION) message, and displayed in the *QGroundControl* **Setup > Summary** airframe panel:

![Firmware info](../../assets/gcs/qgc_setup_summary_airframe_firmware.jpg)

These are extracted at build time from the active *git tag* for your repo tree. The git tag should be formatted as `<PX4-version>-<vendor-version>` (e.g. the tag in the image above was set to `v1.8.1-2.22.1`).

:::warning
If you use a different git tag format, versions information may not be displayed properly.
:::


