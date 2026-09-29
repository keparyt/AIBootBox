# Development Guidance

## Guiding rule

Build the smallest reliable appliance first.

Do not put model downloading, chat UI, agent orchestration and hardware setup into one giant bootstrap script.

## Implementation language

Python is a reasonable first choice for the AIBootBox controller.

Use native Linux tools where they are the correct authority:

- NetworkManager/nmcli;
- systemctl;
- mount;
- nvidia-smi;
- Docker CLI when needed.

Do not create a second service manager inside Python.

## Logging

Use structured logs.

Each boot action should provide:

- timestamp;
- subsystem;
- state;
- severity;
- event ID;
- human-readable message.

Example:

~~~text
INFO  NETWORK  WIFI_CONFIG_FOUND
INFO  NETWORK  CONNECTING
INFO  NETWORK  LINK_READY
INFO  GPU      NVIDIA_READY
INFO  STORAGE  DATA_MOUNTED
INFO  AI       OLLAMA_READY
INFO  AI       LITELLM_READY
INFO  UI       OPENWEBUI_READY
INFO  SYSTEM   AIBOX_READY
~~~

Never log passwords, API keys or session secrets.

## Configuration

Validate configuration at startup and fail early with useful messages.

Distinguish:

- absent;
- invalid;
- valid;
- partially valid;
- secret unavailable.

## Idempotency

Installation and configuration commands should be safe to run repeatedly.

Examples:

- no duplicate firewall rules;
- no duplicate mounts;
- no duplicate NetworkManager connections;
- no destructive overwrite of unrelated configuration;
- no unnecessary model re-download.

## Testing

### Unit

Test configuration parsing, validation, state transitions and network decision logic.

### Integration

Use a disposable VM or dedicated test machine to verify systemd, NetworkManager, storage detection and service ordering.

### Hardware

Test:

- NVIDIA GPU;
- no-GPU machine;
- Ethernet-only;
- Wi-Fi-only;
- missing data drive;
- missing config partition.

## Reproducibility

Record tested versions of:

- Debian;
- kernel;
- NVIDIA driver;
- CUDA runtime/toolkit;
- Docker/container runtime;
- Ollama;
- Open WebUI;
- LiteLLM.

Do not build releases solely against moving latest tags.

## Pull requests

Prefer one subsystem per change:

- boot;
- network;
- storage;
- GPU;
- AI;
- API;
- dashboard;
- installer.

Update documentation with interface changes.

## Diagnostics first

A broken appliance should answer:

~~~text
Which boot stage failed?
What network is active?
What storage is mounted?
What GPU is detected?
Which service failed?
What port is listening?
What should the user do next?
~~~

