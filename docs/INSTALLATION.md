# Installation Guidance

This document defines the intended installation process. The actual automated installer is a later roadmap item.

## Hardware model

AIBootBox is designed around a PC that can boot UEFI from USB.

The first development target is an NVIDIA/CUDA-capable x86-64 machine.

## Recommended disk model

For the first prototype, install Debian directly onto the USB boot device rather than using a read-only Live image with persistence.

Suggested layout:

~~~text
USB HDD/SSD
├── EFI       FAT32   ~512 MiB
├── CONFIG    FAT32   ~1 GiB
├── ROOT      ext4    ~40-80 GiB
└── DATA      ext4    remaining space
~~~

The bootloader must be installed on the USB device itself.

## Installation sequence

1. Boot the Debian installer from a separate installer USB.
2. Select the intended AIBootBox USB device carefully.
3. Install Debian 13 minimal.
4. Install the bootloader to the AIBootBox USB device.
5. Configure basic networking for the installation process.
6. Install AIBootBox.
7. Create and mount the CONFIG partition.
8. Create and mount the DATA filesystem.
9. Install and verify NVIDIA support when enabled.
10. Install the AI services.
11. Enable the AIBootBox systemd target.
12. Reboot with the normal host disk still present.
13. Select the AIBootBox USB from UEFI.
14. Verify the complete boot sequence.

## Important installer safety rule

The installer must clearly display the target disk model, capacity and serial when available.

The first implementation should never automatically choose a removable disk by position such as /dev/sdb.

## First boot

First boot performs:

~~~text
Configuration discovery
        ↓
Network setup
        ↓
Storage validation
        ↓
GPU validation
        ↓
Service startup
        ↓
Health checks
        ↓
READY
~~~

## Rebuild principle

A user should eventually be able to reinstall ROOT while preserving DATA.

That means:

~~~text
ROOT destroyed
   ↓
ROOT recreated
   ↓
mount existing DATA
   ↓
models remain available
~~~

## Release artifacts

A future release should provide one or more of:

- installer scripts;
- a reproducible image builder;
- a prebuilt boot image;
- checksum files;
- version manifest;
- hardware compatibility notes.

The project should not call a generated image reproducible until the exact package and configuration inputs are recorded.

