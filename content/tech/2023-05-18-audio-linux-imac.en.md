 ---
title: Debug audio issue during sleep mode for Arch Linux on 2019 iMac
tags: ['linux']
date: 2023-05-18
---

I am running Arch Linux with T2 kernel on a 2019 iMac. The audio does not work until I apply patches from  [snd_hda_macbookpro](https://github.com/davidjo/snd_hda_macbookpro). However, I am still getting no sound if iMac goes into sleep.

### Brief debugging

Going through kernel messages and checking PCI devices, indicate that after machine resuming, the audio device is lost or does not turn on. The device id is reset to 0x10138409 which is not a subsystem id but the PCI device id of the main sound device. This makes our patch not work at all since no access to the actual codec device.

### Different sleep modes

This iMac supports both s2idle and deep (s3) sleep mode. I tried to switch to s2idle mode. Since s2idle may not power down the hardware competely, the audio issue is gone. However I am running into other issue with s2idle sleep. One of the issues is that the USB controller for TB3 cannot wake up or bring back to D1 state. I tried to set quirks in kernel parameters to force this USB controller to not go into D3 cold, but this cause I lose my USB ports right away.

### Trying with Hibernation

After frustration of making s2idle mode function, and also realizing s2idle mode itself is at disadvantage over s3 mode since it is not a true sleep mode but a software enabled idle mode. I started to give hibernation a try.

iMac works with hibernation without any problem. Audio is perfectly working after resuming. I am definitely not fond of how hibernation works though. It is basically writing your current state into hard drive and reboots the whole machine when you wake it up. The wait time makes this feature like decade old and feel very obsolete in my modern iMac.

### Going back to fix S3 sleep mode

I did get inspired with my root causing why audio failed to work for s3 sleep mode from my experience with hibernation. Investigation led me to believe that audio codec chip is not fully woke up when machine resume. After [discussion with the snd_hda_macbookpro developer](https://github.com/davidjo/snd_hda_macbookpro/issues/87), I am able to force the module to go through a completely boot setup when machine resume and that fixes the issue. The PR is [here](https://github.com/davidjo/snd_hda_macbookpro/pull/90).

The only downside is that there can be a significant latency when resuming if your machine is slow since audio device need to re-configured completely, just as what machine did when booting up. But it makes no difference in resume latency on this powerful 2019 iMac.

### Rethinking the sleep

We also find out an (elegant) example done by official hardware developers for a similar audio codec device cs42l42. The sleep/resume function seems relative straightforward by resetting devices to bring it back to correct power state. However, this requires exact hardware information and unfortunately is not the case for this Apple proprietary audio codec device cs42l83.
