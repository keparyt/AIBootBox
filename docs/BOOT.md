# Boot and Startup Flow

## Goal

AIBootBox should boot from USB and reach a usable AI service without requiring a graphical Linux login.

The preferred mode is headless appliance mode.

## Boot sequence

~~~text
UEFI
 ↓
EFI System Partition
 ↓
Debian kernel + initramfs
 ↓
systemd
 ↓
AIBootBox preflight
 ↓
configuration discovery
 ↓
network
 ↓
storage
 ↓
GPU check
 ↓
AI service target
 ↓
READY
~~~

## No-login model

A graphical session is not required. The Linux system may remain at a console login prompt while its AI services are already running.

This minimizes resource use and makes the system remotely accessible.

A later kiosk mode may launch a browser directly into Open WebUI, but kiosk mode must remain optional.

## Boot controller responsibilities

1. Load and validate configuration.
2. Find the configuration partition.
3. Detect usable Ethernet.
4. Detect a Wi-Fi adapter when needed.
5. Apply configured Wi-Fi.
6. Offer interactive Wi-Fi setup when configuration is unavailable.
7. Verify route and DNS if internet access is required.
8. Mount the AI data filesystem.
9. Verify GPU capability if requested.
10. Start the AI service target.
11. Report status and LAN endpoints.
12. Write structured logs.

## Interactive mode

When a local display and keyboard are available:

~~~text
AIBootBox Network Setup

1. HomeWiFi       -42 dBm
2. StudioWiFi     -58 dBm
3. Other...

Select: _
~~~

Then ask for the password and verify the connection before starting AI services.

Interactive mode should not require a full desktop environment.

## Non-interactive mode

~~~text
CONFIG/wifi.yaml
      ↓
validate
      ↓
connect
      ↓
health check
      ↓
continue
~~~

If the connection fails, retry according to policy and remain in a recoverable network/debug state.

## systemd model

The project should provide an aggregate aibox.target and make prerequisites explicit.

The important contract is:

~~~text
network ready
     ↓
storage ready
     ↓
health checks
     ↓
AI services
~~~

## Recovery

Provide a debug/recovery path that can:

- disable AI services;
- rerun network setup;
- print detected storage;
- print GPU information;
- export diagnostics.

Recovery commands must never delete models or project data.

