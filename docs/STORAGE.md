# Storage Architecture

## Goal

Keep the operating system separate from large AI data so the OS can be rebuilt without re-downloading the model library.

## Starting partition layout

~~~text
USB appliance disk
├── EFI       ~512 MiB
├── CONFIG    ~1 GiB FAT32
├── ROOT      ~40-80 GiB ext4
└── DATA      remaining space
~~~

This is a starting layout rather than a mandatory permanent scheme.

## Roles

### EFI

Contains the boot files and keeps the bootloader self-contained on the AIBootBox device.

### CONFIG

Small FAT32 area editable from common desktop operating systems.

Contains portable setup files only.

### ROOT

Contains Debian, AIBootBox, system packages, service definitions and normal system state.

### DATA

Contains large mutable AI data:

~~~text
DATA/
├── models/
│   ├── ollama/
│   ├── gguf/
│   └── ...
├── projects/
├── datasets/
├── embeddings/
├── docker/
├── cache/
└── backups/
~~~

## Second-drive mode

AIBootBox should also support:

~~~text
USB #1
  Debian + AIBootBox

USB #2
  models + projects + datasets
~~~

This allows a fast boot SSD and a larger model HDD/SSD.

## Stable mount contract

Do not assume device names such as /dev/sdb.

Use filesystem UUIDs or labels. Prefer a stable application mount point such as:

~~~text
/srv/aibox-data
~~~

## HDD considerations

A USB HDD is acceptable for the first prototype. An SSD is preferable for OS boot, container startup, model loading and metadata-heavy workloads.

Once a model is resident in RAM/VRAM, disk throughput is much less dominant than during loading.

## Swap

Avoid designing around heavy swap on a mechanical USB HDD.

Swap policy should remain configurable. Systems with sufficient RAM should prefer keeping the inference workload responsive.

## Safety

The installer must clearly distinguish:

- the installation target;
- the AI data target;
- unrelated disks.

It must never silently repartition every removable device.

