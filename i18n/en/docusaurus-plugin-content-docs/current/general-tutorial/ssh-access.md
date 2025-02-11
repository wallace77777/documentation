---
sidebar_label: "SSH Usage Tutorial"
sidebar_position: 10
---

# SSH Usage Tutorial

Radxa pre-integrates SSH in the official linux image to facilitate remote access to the terminal. This guide uses ROCK 5A as an example, and the operations of other SBCs are similar.

## Necessary Preparation

- ROCK 5A
- RJ45 Network Cable
- PC
- Router / Switch

Connect ROCK 5A to a router / switch on the same network segment as the host through a network cable.

### View SSH Service Status

After the ROCK 5A is started, you can check the status of the SSH service through the following command:

```bash
	sudo service ssh status
```

If the SSH service is abnormal or uninstalled, you can restart or reinstall it with the following commands:  
Restart the service:

```bash
    service sshd restart
```

Re-install：

```bash
    sudo apt-get update
    sudo apt-get install ssh
```

### View IP Adress

#### View via Command

Open the terminal via `Ctrl + Alt + T`, enter the following command in the terminal to view the IP address

```bash
    ip a
```

The IP address in the same network segment as the host is the IP address required for SSH connection, as shown in the following output `192.168.42.100` is the IP address we need:

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 56:ae:03:82:18:51 brd ff:ff:ff:ff:ff:ff
    inet 192.168.42.100/24 brd 192.168.42.255 scope global dynamic noprefixroute eth0
       valid_lft 43176sec preferred_lft 43176sec
    inet6 fe80::8e:d164:96ea:40b6/64 scope link noprefixroute
       valid_lft forever preferred_lft forever

```

#### Use Angryip to find the IP address of the ROCK 5A

This method can be used to view the ip address when the SBC cannot be directly operated to obtain the ip address without a screen of remotely.

- First, the host PC needs to download [Angryip](https://angryip.org/download/), and then make sure the host PC and ROCK 5A are in the same LAN.

- Open Angryip, select the IP range, the range is 192.168.2.0 - 192.168.2.255 (select the network segment where the host and SBC are located). Click Start, as shown in the picture.

- ![Angryip](/img/configuration/ssh-Angryip.webp)

- Ctrl + F Search `rock` keyword, find the IP address of ROCK 5A.

## Connect

- Open the host terminal, and use the `ping` command to check whether they are on the same network segment:

```bash
    ping ROCK 5A's ip address
```

In the case of connectivity, `ping` results should be normal.

- Using SSH login ROCK 5A

```bash
ssh name@ROCK 5A's IP address
For example: ssh radxa@192.168.0.1
```

- If **Local Domain** is supported, you can use the following command instead of scanning the IP address of ROCK 5A.

```bash
ping rock-5a.local
ssh rock@rock-5a.local
```

After successful connection, the terminal will switch to the remote terminal of ROCK 5A, as shown in the figure below:

![ssh connect](/img/configuration/ssh-connect.webp)

Now you can operate ROCK 5A via SSH.
