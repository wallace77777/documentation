---
sidebar_label: "Overview"
sidebar_position: 3
---

# ROCK 3C Overview

The ROCK 3C is an SBC based on the RK3566 SoC.
Equipped with a Quad-Core Cortex-A55 ARM processor, 32-bit 3200Mb/s LPDDR4 on board, 1920x1080@60 HDMI support,
MIPI DSI, MIPI CSI, 3.5mm audio jack with microphone support, USB port, Gigabit Ethernet, PCIe 2.0, 40-pin IO expansion header.

## Close Look

![3C top view](/img/rock3/Rock3C-top-800px.webp)![3C bottom view](/img/rock3/Rock3C-bottom-800px.webp)

## Features

|        Model        | ROCK 3 Model C                                                                                         |
| :-----------------: | :----------------------------------------------------------------------------------------------------- |
| SoC<br/>CPU<br/>GPU | **RK3566**<br/>Quad-core Cortex-A55, frequency 1.6Ghz<br/>Mali G52                                     |
|       Memory        | 1/2/4GB LPDDR4 2112MT/s                                                                                |
|       Storage       | eMMC<br/>μSD card: supports up to 128 GB μSD<br/>M.2 2230 SSD: supports up to 1TB                      |
|       Display       | HDMI: up to 1920x1080@60<br/>_OR_ MIPI DSI 2-lane display                                              |
|        Audio        | 3.5mm jack with mic<br/>HD codec that supports up to 24-bit/96KHz audio.                               |
|       Camera        | MIPI CSI 2 lanes via FPC connector, support up to 8 MP camera.                                         |
|      Wireless       | On-board wireless chip with Wi-Fi 5 (802.11ac) and Bluetooth 5.0 support                               |
|         USB         | USB 2.0 OTG X1, hardware switch for host/device switch<br/>USB 3.0 HOST X1<br/>USB 2.0 HOST X2         |
|      Ethernet       | Supports Gigabit Ethernet. Power over Ethernet (PoE) support available with an additional HAT purchase |
|         IO          | [40 pin GPIO](../hardware/rock3c-gpio)                                                                 |
|        Power        | USB type C 5V, 5V/3A is suggested when using without SSD, 5V/5A is suggested when using with SSD.      |
|        Size         | 85mm x 56mm                                                                                            |
