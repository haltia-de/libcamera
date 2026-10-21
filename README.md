# libcamera

[libcamera](https://libcamera.org/) 0.7.1 packaged for Alpine 3.24 armv7
under Alpine's own names (`libcamera`, `libcamera-ipa`, `libcamera-gstreamer`,
`libcamera-tools`, …), as version `0.7.1_p1`, so `apk` prefers it to
Alpine's 0.7.1-r1.

It carries three patches for the front camera of the Haltia tablet (Lenovo
YOGA Tablet 10, [device-lenovo-b8000h](https://github.com/haltia-de/device-lenovo-b8000h)):

1. the MediaTek MT6589 camera interface (`mt6589-seninf`) in the simple
   pipeline handler, with the software ISP;
2. the OmniVision OV9760 sensor: gain model, black level, pixel size;
3. a tuning file `ov9760.yaml` for the software ISP.

The sensor driver writes 8-bit Bayer (`SBGGR8`), which the software ISP
already handles. The patches go to libcamera upstream too; once a release
with them reaches Alpine, this repository is retired.

Packages come from the signed Haltia apk repository (haltia-de/packages).
License: LGPL-2.1-or-later (libcamera's library; its tools are
GPL-2.0-or-later).
