<p align="center">
    <picture>
        <source media="(prefers-color-scheme: dark)" srcset=".meta/logo-dark.png" width="40%" />
        <source media="(prefers-color-scheme: light)" srcset=".meta/logo-light.png" width="40%" />
        <img alt="T3 Foundation" src=".meta/logo-light.png" width="40%" />
    </picture>
</p>

# T3 Gemstone O1 Edge AI Firmware

  [![T3 Foundation](./.meta/t3-foundation.svg)](https://www.t3vakfi.org/en) [![Documentation](https://img.shields.io/badge/Documentation-gray?style=flat&logo=Mintlify)](https://docs.t3gemstone.org/en/boards/o1/ai/introduction) [![Build Debian Package](https://github.com/t3gemstone/firmware-o1-edge-ai/actions/workflows/build-deb.yml/badge.svg)](https://github.com/t3gemstone/firmware-o1-edge-ai/actions/workflows/build-deb.yml)

## What is it?

This repository contains the firmware required for Edge AI on T3 Gemstone O1 development board.
For detailed information, please visit https://docs.t3gemstone.org/en/boards/o1/ai/introduction .

The firmware is distributed as the `t3-edgeai-firmware` Debian package, which installs it into
`/lib/firmware` together with the `j722s-*-fw` symlinks that the remoteproc driver loads.
The symlinks are listed in `packaging/links` and the package metadata is in `packaging/control.in`.

During installation the `postinst` maintainer script also appends
`k3-am67a-t3-gem-o1-edgeai-apps.dtbo` to the `overlays=` line of `/boot/uEnv.txt`, so that the
Edge AI device tree overlay is loaded at boot. The edit is idempotent: reinstalling or upgrading
the package never duplicates the entry, and if the entry is already there it is left untouched.

#### 1. Install Task on the host computer.

```bash
 sh -c "$(curl --location https://taskfile.dev/install.sh)" -- -d -b ~/.local/bin
```

#### 2. Build the Debian package.

```bash
 task build
```

The package is written to `build/t3-edgeai-firmware_11.0.0_arm64.deb`. To build another version, run `task build PKG_VERSION=11.0.1`.

#### 3. Install the package on the board and reboot so that the Edge AI overlay is applied and the remote cores load the new firmware.

```bash
 sudo apt install ./t3-edgeai-firmware_11.0.0_arm64.deb
 sudo reboot
```

### Releases

The [Build Debian Package](.github/workflows/build-deb.yml) workflow builds the package on every push and pull request and uploads it as a workflow artifact.
Pushing a tag such as `v11.0.1` builds version `11.0.1` and attaches the package and its `SHA256SUMS` to a GitHub release.
