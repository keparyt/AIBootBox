# Security Model

AIBootBox is designed primarily for self-hosted, trusted-LAN deployments. LAN exposure still requires deliberate authentication and boundaries.

## Threat model

Assume:

- other devices can exist on the LAN;
- some LAN users may be untrusted;
- the USB may be physically accessed;
- Wi-Fi credentials may be stored on removable media;
- AI services can process files and future agents may execute commands.

## Network boundary

~~~text
Internet
   X
   │
Firewall
   │
LAN
 ├── Web UI
 └── API gateway
       │
       └── private AI backends
~~~

Public internet exposure is not part of the default design.

## Authentication

Require authentication for:

- Open WebUI administration;
- LAN API;
- future AIBootBox administration APIs.

API keys must never appear in normal diagnostics or Git.

## Ollama

Keep Ollama on localhost or another protected interface by default.

Use LiteLLM as the normal remote API boundary.

## Wi-Fi credentials

The MVP accepts a plaintext Wi-Fi configuration file for portability.

Threat:

~~~text
USB stolen
   ↓
wifi.yaml copied
   ↓
Wi-Fi credential exposed
~~~

Future options can include encrypted configuration, one-time pairing or secure local enrollment.

## Agent execution

Future coding/agent services are a separate security domain.

Agents should run with least privilege and should not receive unrestricted root access by default.

Project workspaces should be isolated from system paths.

## Model downloads

Treat model files as external content.

Record source and model identifier. When a reliable checksum exists, record and verify it.

Do not execute arbitrary files from model directories.

## Updates

Keep separate release/upgrade policies for:

- Debian security updates;
- NVIDIA drivers;
- AI runtimes;
- Open WebUI;
- LiteLLM;
- AIBootBox itself.

Releases should record tested version combinations.

