# Scripts

Scripts are split by lifecycle:

- install: install/configure a running Debian system;
- image: build or provision a bootable AIBootBox image;
- diagnostics: collect safe troubleshooting information;
- development: prepare a development environment.

Scripts should be idempotent and should support a dry-run or confirmation mode for destructive operations.

No script should assume that a removable disk is /dev/sdb or that network interface names are fixed.

