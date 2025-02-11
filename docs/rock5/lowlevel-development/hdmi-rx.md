---
sidebar_label: "HDMI RX 使用"
sidebar_position: 30
---

# HDMI RX的使用

ROCK 5B 有一个 HDMI RX 接口，支持标准的 HDMI 2.0 协议，可以支持高达 2160p@60Hz 的视频输入。

- 准备 ROCK 5B
- 准备 Micro HDMI 线材

## 使用步骤

HDMI RX 设备接入 ROCK 5B 后，将被注册为内核中的视频设备，生成的节点为 /dev/video0，可以使用 v4l2-ctl 命令获取设备信息并捕获帧。

- 检查设备信息

```bash
$ v4l2-ctl -d /dev/video0 -D
Driver Info:
       Driver name      : rk_hdmirx
       Card type        : rk_hdmirx
       Bus info         : fdee0000.hdmirx-controller
       Driver version   : 5.10.66
       Capabilities     : 0x84201000
               Video Capture Multiplanar
               Streaming
               Extended Pix Format
               Device Capabilities
       Device Caps      : 0x04201000
               Video Capture Multiplanar
               Streaming
               Extended Pix Format
```

- 确认分辨率和图像格式

```bash
$ v4l2-ctl -d /dev/video0 --get-fmt-video
Format Video Capture Multiplanar:
       Width/Height      : 3840/2160
       Pixel Format      : 'NV12' (Y/CbCr 4:2:0)
       Field             : None
       Number of planes  : 1
       Flags             : premultiplied-alpha, 0x000000fe
       Colorspace        : SMPTE 170M
       Transfer Function : Default
       YCbCr/HSV Encoding: Unknown (0x000000ff)
       Quantization      : Default
       Plane 0           :
          Bytes per Line : 3840
          Size Image     : 12441600
```

- 获取数字视频时序

```bash
$ v4l2-ctl -d /dev/video0 --get-dv-timings
DV timings:
       Active width: 3840
       Active height: 2160
       Total width: 4120
       Total height: 2250
       Frame format: progressive
       Polarities: -vsync -hsync
       Pixelclock: 297000000 Hz (32.04 frames per second)
       Horizontal frontporch: 88
       Horizontal sync: 44
       Horizontal backporch: 148
       Vertical frontporch: 8
       Vertical sync: 10
       Vertical backporch: 72
       Standards:
       Flags:
```

- 通过设置分辨率和像素格式捕获图像文件

将 HDMI RX 设置为 4k 显示器并连接到 HDMI RX。

```bash
$ v4l2-ctl --verbose -d /dev/video0 --set-fmt-video=width=3840,height=2160,pixelformat='NV12' --stream-mmap=4 --stream-skip=3 --stream-count=5 --stream-to=/home/rock/hdmiin4k.yuv --stream-poll
```

- 检查图片文件

在 Windows 使用 7yuv 工具

在 Linux 使用 ffplay 工具

```bash
$ ffplay -f rawvideo -video_size 3840x2160 -pixel_format nv12 /home/rock/hdmiin4k.yuv
```

- 检测音频功能

当你的 HDMI RX 有音频输入时，你可以用 arecord 从 HDMI RX 端口获得音频并在耳机上播放。

```bash
$ arecord -l
**** List of CAPTURE Hardware Devices ****
card 2: rockchipes8316 [rockchip-es8316], device 0: fe470000.i2s-ES8316 HiFi es8316.7-0011-0 [fe470000.i2s-ES8316 HiFi es8316.7-0011-0]
 Subdevices: 1/1
 Subdevice #0: subdevice #0
card 3: rockchiphdmiin [rockchip,hdmiin], device 0: fddf8000.i2s-dummy_codec hdmiin-dc-0 [fddf8000.i2s-dummy_codec hdmiin-dc-0]
 Subdevices: 1/1
 Subdevice #0: subdevice #0
```

你可以看到，HDMI RX（HDMI IN）的声卡号是 2，你可以运行下面的命令，在 HDMI RX 有音频输入时，录制和播放音频。

```bash
# get 5 seconds audio file
$ arecord -Dhw:3,0 -d 5 -f cd -r 44100 -c 2 -t wav /tmp/hdmiin_audio.wav
# play on headphone
$ aplay -D plughw:2,0 /tmp/hdmiin_audio.wav
```
