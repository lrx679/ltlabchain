# Debian Packaging

This file records notes for producing Debian packages from the LTLab Chain
source tree.

Release packaging should be driven by LTLab Chain owned infrastructure and
signing keys. Do not reuse inherited upstream release buckets, PPAs, CI secrets,
or upload targets.

## Building Packages Locally

Use an Ubuntu build environment with Go and Debian packaging tools installed:

```shell
sudo apt-get install build-essential golang-go devscripts debhelper python3-paramiko
```

Create a source package:

```shell
go run build/ci.go debsrc -workdir dist
```

Build the package from the generated source package directory:

```shell
cd dist/<source-package>
dpkg-buildpackage
```

Built packages are written under `dist/`.

Before publishing packages, verify package names, maintainer metadata, signing
keys, checksums, and upload destinations for the target LTLab Chain release.
