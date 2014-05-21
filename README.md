# Netkit UML build

## Introduction

Netkit development has stalled since some time and the scripts to build the fs
and the kernel are now broken due to a bug in deboostrap which is not going to
be fixed (the fs is based on the now deprecated debian Sid).

This project proposes a generic way to build and expand kernel and image for
Netkit, levering build tools in debian. More specifically:

- kernel is build from source using debian package and patched with netkit
  patchs.
- rootstrap builds the fs with new modules dedicated to netkit.

At the present time:
- only wheezy is supported.
- only i386 kernel and fs builds are supported.

## Warning

The kernel and the fs ONLY work with a modified version of `netkit-core`.

## Installation

The build is developped and tested on debian Wheezy.

It is recommanded to use a non-critical virtual machine, as the build script
requires root user.

The build requires several tools which can be installed with the following
commands (using root user):

    apt-get install build-essentials rootstrap

Launch the build with the command make in the root directory of the project.



## Configuration

The following files can help an user to configure the build:

- `fs/filesystem-tweaks/` directory contains files which are copied in the fs.
- `fs/disabled-services` desactivates `/etc/init.d` scripts in the fs.
- `fs/packages-list` lists the packages to installed in the fs.
- `fs/debconf-package-selections` contains the configuration applied to the 
  packages.
- `fs/packages-list` lists the packages to installed in the fs.
- `kernel/config.i386` is the configuration of the kernel.
- `kernel/patches/` directory contains the patches applied before kernel
  build.

## Bug

* The filesystem uses the sparse mode. On encrypted partition, the disk access can be very slow (very!).
