---
sidebar_label: "功能介绍"
sidebar_position: 0
---

# 简介

`rsetup` 是瑞莎开发的系统设置工具。

要启动 `rsetup`，请直接在终端中执行 `rsetup` 命令，或从系统菜单中找到 `rsetup` 应用：

:::caution
`rsetup` 运行时需要管理员权限，请确保你拥有 `sudo` 权限。
:::

```bash
┌──────────────────────────────────┤ RSETUP ├──────────────────────────────────┐
│ Please select an option below:                                               │
│                                                                              │
│                             System Maintenance                               │
│                             Hardware                                         │
│                             Overlays                                         │
│                             Connectivity                                     │
│                             User Settings                                    │
│                             Localization                                     │
│                             About                                            │
│                                                                              │
│                     <Ok>                         <Cancel>                    │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

:::caution
上述列出的选项仅供参考，请以实际程序输出为准。
:::

## 界面操作

使用 `Up` 和 `Down` 键来切换选项。当前选中的项目会高亮显示。

使用 `Tab` 键来切换菜单区域和命令区域。

使用 `Left` 和 `Right` 键来切换 `<Ok>` 和 `<Cancel>` 命令。使用 `Enter` 键来执行选中的命令。  
当命令没有被选中时按下 `Enter` 键，会立即执行 `<Ok>` 命令。

使用 `Space` 键选择选项。选项会包含 `[ ]` （多项选择） 或 `( )` （单项选择）前缀来显示当前的选择状态。

:::caution
使用 `Enter` 键不会选择选项。它会直接执行 `<Ok>` 命令。
:::

使用 `Esc` 键回到前一个菜单。

## System Maintenance

本菜单包含以下选项：

```bash
┌──────────────────────────────────┤ RSETUP ├──────────────────────────────────┐
│ System Maintenance                                                           │
│                                                                              │
│                         System Update                                        │
│                         Update Bootloader                                    │
│                         Update SPI Bootloader                                │
│                         Update eMMC Boot partition                           │
│                                                                              │
│                     <Ok>                         <Cancel>                    │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

### System Update

本选项会更新你的系统中安装的软件包到最新状态。

由于 RadxaOS 提供了一些和上游 Debian 有所冲突的软件包，为了保证系统一致性，建议通过此方式进行系统更新。

### Update Bootloader

:::caution
更新启动器有系统无法启动的风险。请在数据已备份的前提下进行本操作。
:::

本选项会更新根文件系统所在磁盘上的启动器。

:::warning
本选项仅适用于启动设备和系统设备是同一设备的场景。  
如果你是在安装完系统后直接启动的话（也即，没有使用定制化的启动选项），则可以使用本选项。
:::

### Update SPI Bootloader

:::caution
更新启动器有系统无法启动的风险。请在数据已备份的前提下进行本操作。
:::

本选项会更新 SPI Flash 上的启动器。

:::tip
本选项适用于需要从 NVMe 固态硬盘或 USB 大容量存储设备进行启动的场景。  
并非所有产品均支持 SPI 启动。
:::

### Update eMMC Boot Bootloader

:::caution
更新启动器有系统无法启动的风险。请在数据已备份的前提下进行本操作。
:::

本选项会更新 eMMC Boot 分区上的启动器。

:::tip
本选项适用于需要从 NVMe 固态硬盘或 USB 大容量存储设备进行启动的场景。  
并非所有产品均支持 eMMC Boot 分区启动。  
基于 Rockchip SoC 的产品不支持 eMMC Boot 分区启动。
:::

## Hardware选项

在这个菜单上可以修改一些外围设备，下面是对它们的介绍。

```
┌──────────────────────────────────┤ RSETUP ├──────────────────────────────────┐
│ Manage on-board hardware                                                     │
│                                                                              │
│                      Video capture devices                                   │
│                      GPIO LEDs                                               │
│                      Thermal governor                                        │
│                      Configure DSI display mirroring                         │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                     <Ok>                         <Cancel>                    │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Video capture devices选项

如果你有连接一个视频设备，这个选项可以帮助你测试该设备。

### GPIO LEDs选项

在这个选项上，你可以改变板子上的电源LED和用户LED的状态（有时只有一个）。
详细情况请参考[LED设置](led)。

### Thermal governor选项

该模块用于调整风扇和其他温度选项。
一般来说，当使用直流风扇或不使用风扇时，选择`power_allcator`，当使用PWM风扇时，选择`step_wise`。  
**注意：如果你将风扇直接连接到GPIO的5V和GND引脚上，它将不受控制且一直工作。**

### Configure DSI display mirroring选项

如果在同时使用HDMI显示器和DSI显示器时，DSI显示器有一些问题，启用这个选项会有所帮助。

## Overlays选项

该选项用于配置设备树Overlays，请查看[设备树配置](devicetree)获取更多信息。

## Connectivity选项

该选项与[网络设置](../network)类似。

## User Settings选项

可以在这个选项上更改当前登录账户的 `Password` 和设备的 `Hostname`。

## Localization选项

在这个选项中, 你可以改变 `Timezone` `Locale` `Keybaed layout` `Wi-Fi Country`.

## About选项

这里是 `rsetup `的相关信息。
