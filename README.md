# AIBootBox

AIBootBox is a portable, bootable Linux AI appliance designed to turn a normal x86-64 PC into a temporary local AI server.

The primary use case is a USB-connected Debian installation that:

- boots independently of the host operating system;
- starts without requiring a desktop login;
- detects and initializes NVIDIA GPU acceleration;
- connects to Ethernet or Wi-Fi before starting AI services;
- optionally reads Wi-Fi credentials from a user-editable USB configuration file;
- falls back to an interactive Wi-Fi setup when no configuration is supplied;
- mounts a dedicated AI data area for models, projects, datasets and containers;
- starts the AI stack automatically;
- provides an LM Studio-like browser interface for chatting and model management;
- exposes an authenticated OpenAI-compatible API to the LAN;
- can eventually aggregate multiple local AI engines and agent services behind one endpoint.

## Design goal

AIBootBox should feel like an AI appliance, not a general-purpose Linux desktop.

~~~text
POWER ON
   ↓
UEFI / USB
   ↓
Debian
   ↓
AIBootBox boot controller
   ↓
Network + storage + GPU health
   ↓
AI services
   ↓
READY
~~~

## Baseline

- Debian 13 "trixie"
- Minimal/headless installation by default
- NVIDIA driver plus CUDA-compatible userspace
- Ollama as the first local model runtime
- Open WebUI as the primary user interface
- LiteLLM as the LAN/API gateway
- systemd as boot/service orchestration
- NetworkManager and nmcli for network management
- Docker only where containerization is useful

Debian 13 is the current stable release as of September 2026.

## Core principles

1. Boot independently.
2. Fail safely.
3. Keep the boot path small.
4. Separate control from inference.
5. Keep raw backends private where possible.
6. Make AI data replaceable and portable.
7. Detect hardware by capability rather than hard-coding one machine.
8. Prefer declarative configuration.
9. Never allow an optional AI component to brick the appliance.
10. Optimize for temporary full-power AI workloads.

## Documentation

- Architecture: docs/ARCHITECTURE.md
- Boot and startup: docs/BOOT.md
- Network and Wi-Fi: docs/NETWORK.md
- AI stack: docs/AI-STACK.md
- Storage: docs/STORAGE.md
- Security: docs/SECURITY.md
- Roadmap: docs/ROADMAP.md
- Development: docs/DEVELOPMENT.md
- Project structure: docs/PROJECT_STRUCTURE.md

## MVP

The first usable release should:

1. boot from USB using UEFI;
2. start Debian without a graphical login;
3. find the AIBootBox configuration partition;
4. configure Ethernet or Wi-Fi;
5. provide an interactive Wi-Fi fallback;
6. verify NVIDIA when GPU mode is enabled;
7. mount AI data storage;
8. start Ollama, Open WebUI and LiteLLM in the correct order;
9. display the LAN URLs and service status;
10. survive reboot without manual intervention.

The custom AIBootBox code should remain small and diagnosable.

## Upstream projects

- Debian: https://www.debian.org/
- NVIDIA CUDA: https://docs.nvidia.com/cuda/
- Ollama: https://ollama.com/
- Open WebUI: https://docs.openwebui.com/
- LiteLLM: https://docs.litellm.ai/
- NetworkManager: https://networkmanager.pages.freedesktop.org/NetworkManager/

Versions used for releases should be pinned and recorded rather than relying on floating latest tags.

