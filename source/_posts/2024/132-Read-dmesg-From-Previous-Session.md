---
title: Read dmesg From Previous Session
date: 2024-03-22 14:16:22
categories: Linux 系统和软件
tags:
abbrlink: 132
standard: 1
references:
  - https://unix.stackexchange.com/questions/181067/how-to-read-dmesg-from-previous-session-dmesg-0
---
If your system uses `journalctl` then you can easily get the kernel messages (dmesg log) from prior shutdown/crash (in a `dmesg -T` format) through the following.

Options:

- `-k` Show kernel messages.
- `-b <boot_number>` How many reboots ago 0, -1, -2, etc.
- `-o short-precise` Print in a `dmesg -T` format.
- `-p <priority>` Filter by priority output (4 to filter out notice and info).

{% note info %}
There is also an `-o short` and `-o short-iso` which gives you the date only, and the date-time in ISO format respectively.
{% endnote %}

Commands:

- All boot cycles : `journalctl -o short-precise -k -b all`
- Current boot : `journalctl -o short-precise -k`
- Last boot : `journalctl -o short-precise -k -b -1`
- Two boots prior : `journalctl -o short-precise -k -b -2`

And so on.

The amount of boots you can look back on can be viewed with the following.

```bash
journalctl --list-boot
```

The output of `journalctl --list-boot` looks like the following.

```
-6 cc4333602fbd4bbabb0df2df9dd1f0d4 Sun 2016-11-13 08:32:58 JST—Thu 2016-11-17 07:53:59 JST
-5 85dc0d63e6a14b1b9a72424439f2bab4 Fri 2016-11-18 22:46:28 JST—Sat 2016-12-24 02:38:18 JST
-4 8abb8267e06b4c26a2466562f3422394 Sat 2016-12-24 08:10:28 JST—Sun 2017-02-12 12:31:20 JST
-3 a040f5e79a754b2a9055ac2598d430e8 Sun 2017-02-12 12:31:36 JST—Sat 2017-02-18 21:31:04 JST
-2 6c29e3b6f6a14f549f06749f9710e1f2 Sat 2017-02-18 21:31:15 JST—Sat 2017-02-18 22:36:08 JST
-1 42fd465eacd345f7b595069c7a5a14d0 Sat 2017-02-18 22:51:22 JST—Sat 2017-02-18 23:08:30 JST  
 0 26ea10b064ce4559808509dc7f162f07 Sat 2017-02-18 23:09:25 JST—Sun 2017-02-19 00:57:35 JST
```
