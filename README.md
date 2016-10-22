# tinycore-kernel

Build a [TinyCore Linux](http://tinycorelinux.net/) kernel, modules, and firmware extensions

# Getting Started

Clone this repository.

### Building a kernel from scratch

The default kernel is `3.16.6-tinycore64`

  1. `make kernel`
  2. `cd /tmp/tinycore-kernel-build`
  3. The kernel will be named `vmlinuz64`

### Building default module extensions

Default modules extensions are `all, base, filesystems, ipv6, mtd, netfilter, raid-dm, scsi`.

The `all` extension will contain _all_ the modules.

The `base` extension will contain the _base_ modules which are included of the OS `core.gz/corepure64.gz`.

  1. `make extensions`
  2. `cd /tmp/tinycore-kernel-build`
  3. The module extensions will be named `<module>-3.16.6-tinycore64.tcz`

### Rebuilding the OS (remaster) with new modules

The default OS will be searched for in `/opt/tinycore/6.x/x86_64/release/distribution_files/`.

  1. `make os`
  2. `cd /tmp/tinycore-kernel-build`
  3. The compressed OS will be named `corepure64.gz` and uncompressed OS will be named `corepure64`

### Cleaning up

Many directories and files will be created in the `WORKDIR`, to cleanup, simply type `make clean`.

# Makefile variables

The following variables can be changed at build time, by specifying them as arguments, ex: `make MODULES=ipv6`

  * `MODULES`: List of modules to build
  * `KERNEL`: Kernel version to build
  * `KERNEL_ARCH`: Kernel architecture to build, either `x86_64` or `x86`
  * `KERNEL_SOURCE`: URL of the kernel source package (`.tar.xz or .txz`)
  * `KERNEL_SHA256`: SHA256 hash of the kernel source package
  * `OSDIR`: Local system directory containing the TinyCore OS (`core.gz or corepure64.gz`)
  * `WORKDIR`: Temporary work path to build all the files

# Notes

This is a very early initial release, so it may be buggy, and might not be complete. Please inspect `WORKDIR` after building, if you want to package firmwares or additional modules.

# Contributing

If you find any bugs or issues, please [create an issue](https://github.com/jidoteki/tinycore-kernel/issues/new).

If you want to improve this, please make a pull-request.

# License

[MIT License](LICENSE)

Copyright (c) 2016 Alexander Williams, Unscramble <license@unscramble.jp>
