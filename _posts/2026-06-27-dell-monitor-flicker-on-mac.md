---
layout: post
title: Dell monitor flicker on macs with Apple M processors
date: 2026-06-27
categories: dell
---

I have Dell P2723QE and it flickers when connected to M2 macbook.

Turned out that it is know issue, and a lot of DELL monitors are affected.

The flicker is caused by the monitors LCD panel’s Image Compensation Algorithm (ICA) function repeatedly turning On and Off. 
The YCbCr Video generated from the Apple system is at 17Gray level, it fluctuates between 15Gary and 17Gary. 
Hence it is repeatedly turning On and Off, the ICA which has a threshold setting of 16Gray.

### Dell suggests following actions to fix that (they don't help to fix this issue permanenetly)
- <https://www.dell.com/support/kbdoc/en-id/000331897/apple-m-processors-might-cause-dell-monitors-to-flicker/>
- set refresh rate to constant 60 or 30 Hz
- turn off Auto Adjust Brightness and True Tone 
- use the Light Appearance
- turn off Night Shift 

### For me this helps to fix it temporary (DELL P2723QE)
- Input Color Format - RGB
- USB-C Prioritization - High resolution
- LCD Conditioning for 5 minutes
- Brightness and Contrast 20..40
- Factory Reset

### Permanent fix (the proper one)
- Disable macOS *`Temporal Dithering`* with one of these tools
  - **Stillcolor** (my choise, eyes are happy now) : <https://github.com/aiaf/Stillcolor/>
  - **BetterDisplay** (adjust the color bit-depth sent from the GPU) : <https://github.com/waydabber/BetterDisplay/>

### Affected Products
- Dell 24 Video Conferencing Monitor C2422HE
- Dell 27 Video Conferencing Monitor C2722DE
- Dell 32 Gaming Monitor G3223D
- Dell 34 Video Conferencing Curved Monitor C3422WE
- Dell C1422H
- Dell P1424H
- Dell P2421DC
- Dell P2422HE
- Dell P2423DE
- Dell P2424HEB
- Dell P2721Q
- Dell P2722HE
- Dell P2723DE
- Dell P2723QE
- Dell P2724DEB
- Dell P3221D
- Dell P3222QE
- Dell P3223DE
- Dell P3223QE
- Dell P3424WE
- Dell P5524QT
- Dell P6524QT
- Dell P8624QT
- Dell S2422HZ
- Dell S2722DZ
- Dell S2722QC
- Dell S2723HC
- Dell S3423DWC
- Dell U2421E
- Dell U2421HE
- Dell U2424H
- Dell U2424HE
- Dell U2721DE
- Dell U2722D
- Dell U2722DE
- Dell U2722DX
- Dell U2723QE
- Dell U2723QX
- Dell U2724D
- Dell U2724DE
- Dell U3023E
- Dell U3223QE
- Dell U3223QZ
- Dell U3224KB
- Dell U3421WE
- Dell U3423WE
- Dell U3821DW
- Dell U3824DW
- Dell U4021QW
- Dell U4323QE
- Dell U4924DW
