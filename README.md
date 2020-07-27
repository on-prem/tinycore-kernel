# tinycore-kernel

Build a [TinyCore Linux](http://tinycorelinux.net/) kernel, modules, and firmware extensions

# Getting Started

  1. Install build dependencies: `tce-load -wi git compiletc coreutils bc`
  2. Clone this repository: `git clone https://github.com/jidoteki/tinycore-kernel`

### Building a kernel from scratch

The default kernel is `4.19.134-tinycore64`

  1. `make kernel`
  2. `cd /tmp/tinycore-kernel-build`
  3. The kernel will be named `vmlinuz64`

### Building default module extensions

Default modules extensions are `all, base, filesystems, ipv6, mtd, netfilter, raid-dm, scsi, net-bridging`.

The `all` extension will contain _all_ the modules.

The `base` extension will contain the _base_ modules which are included of the OS `core.gz/corepure64.gz`.

  1. `make extensions`
  2. `cd /tmp/tinycore-kernel-build`
  3. The module extensions will be named `<module>-4.9.66-tinycore64.tcz`

### Rebuilding the OS (remaster) with new modules

The default OS will be searched for in `/opt/tinycore/8.x/x86_64/release/distribution_files/`.

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

# Gotchas

TinyCore Linux kernel compiling comes with a few gotchas. The list below will hopefully help others who may run into these problems when manually compiling.

### Kernel panic on boot

This occurs when you recompile the kernel (ex: `vmlinuz64`) without recompiling the modules and/or module extensions. The `base` modules in the OS (ex: `corepure64.gz`) must be replaced, as well as any module extensions (ex: `ipv6-4.9.66-tinycore64.tcz`).

### Kernel seems to boot, but can't mount loop or disks

The `modules.dep` file should point to `.ko.gz` kernel modules, but Linux compiles them as `.ko` by default. The `Makefile` in this repo gzips all modules, and then performs a `sed` on the newly built `modules.dep` to ensure it also contains `.ko.gz` entries. This `modules.dep` will be rebuilt on boot anyways, but it still needs to be correct initially.

The `kernel.tclocal` symlink is also needed for kernel module extensions to be loaded. See the [TinyCore Custom Kernel wiki page](http://wiki.tinycorelinux.net/wiki:custom_kernel) for more info.

### Newly compiled modules are not being loaded

If you edit the kernel config using `make menuconfig` or other, and add new modules, you will need to create a new extension containing those modules. It is recommended to do that as opposed to adding them directly in the OS. Adding them to the OS can lead to problems in the future if you accidentally "forget" to include them when you update TinyCore.

### Weird things are happening

The default TinyCore Linux kernel is compiled with a set of cosmetic/boot Linux kernel patches, and one patch for AGP. We excluded them in our newer kernel builds (ex: `3.16.38-tinycore64`), but you're better off patching your kernel with those patches if you experience strange system behaviour.

# Notes

Please inspect `WORKDIR` after building, if you want to package firmwares or additional modules.

# Contributing

If you find any bugs or issues, please [create an issue](https://github.com/jidoteki/tinycore-kernel/issues/new).

If you want to improve this, please make a pull-request.

# License

[MIT License](LICENSE)

Copyright (c) 2016-2019 Alexander Williams, Unscramble <license@unscramble.jp>
