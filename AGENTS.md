# AGENTS.md

Guidance for coding agents in this repository. `CLAUDE.md` only includes it.
Project-wide decisions (ADRs), plans and conventions live in
[haltia-de/haltia](https://github.com/haltia-de/haltia) (`docs/conventions.md`
for releases and packages; the camera plan `docs/plans/camera-app.md`).

## What this is

Alpine's `community/libcamera` APKBUILD (`packaging/alpine/`) with the
Haltia tablet's patches, built for armv7 by haltia-de/ci's apk workflow. No
libcamera sources live here: the APKBUILD fetches the release tarball and
checks its sha512. qcam is left out (Qt 6, the longest part of the build).

## Rules

- Keep Alpine's package names and subpackages; the version is Alpine's
  upstream version with a `_pN` suffix, above Alpine's `-rN`.
- Patches are `git format-patch` output against the libcamera release, in
  upstream style (subject prefix, commit message), so they can be sent to
  libcamera-devel as they are. Keep them LF.
- A release is a `vX.Y.Z_pN` tag equal to `pkgver` (with `pkgrel=0`).
- After a release, pin it in device-lenovo-b8000h's
  `linux-port/apps/packages.lock`.
- Check on the tablet: `cam -l`, `cam -c 1 -C 30`,
  `gst-launch-1.0 libcamerasrc ! video/x-raw,width=640,height=480 ! videoconvert ! fakesink`.
  Camera frames may show people: never publish them.

## Build by hand

```sh
docker run --rm --platform linux/arm/v7 -e WANT_ARCH=armv7 \
  -v "$PWD:/src" -v /path/to/haltia-de/ci:/ci:ro alpine:3.24 sh /ci/build-apk.sh
```

(armv7 on x86 needs qemu-arm in binfmt_misc; the build takes a long while
under emulation.)
