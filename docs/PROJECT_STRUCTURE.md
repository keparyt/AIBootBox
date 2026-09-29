# Project Structure

The repository separates machine orchestration, configuration, service deployment, installer/build logic and documentation.

~~~text
AIBootBox/
├── README.md
├── LICENSE
├── SECURITY.md
├── CONTRIBUTING.md
│
├── config/
│   ├── aibox.example.yaml
│   └── wifi.example.yaml
│
├── docs/
│   ├── ARCHITECTURE.md
│   ├── BOOT.md
│   ├── NETWORK.md
│   ├── AI-STACK.md
│   ├── STORAGE.md
│   ├── SECURITY.md
│   ├── ROADMAP.md
│   ├── DEVELOPMENT.md
│   └── PROJECT_STRUCTURE.md
│
├── src/
│   └── aibox/
│       ├── main.py
│       ├── config.py
│       ├── logging.py
│       ├── state.py
│       ├── diagnostics.py
│       ├── boot/
│       │   ├── preflight.py
│       │   ├── network.py
│       │   ├── storage.py
│       │   ├── gpu.py
│       │   └── services.py
│       ├── api/
│       │   ├── app.py
│       │   ├── health.py
│       │   └── status.py
│       └── cli/
│           └── ...
│
├── systemd/
│   ├── aibox-boot.service
│   ├── aibox-network.service
│   ├── aibox-storage.service
│   ├── aibox-health.service
│   ├── aibox.target
│   └── ...
│
├── scripts/
│   ├── install/
│   ├── image/
│   ├── diagnostics/
│   └── development/
│
├── deploy/
│   ├── docker/
│   ├── litellm/
│   └── open-webui/
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── fixtures/
│
└── examples/
    ├── single-gpu/
    ├── wifi/
    └── lan/
~~~

The src/aibox directory owns AIBootBox behavior. It should not become a replacement for systemd, NetworkManager, Docker or the inference engines.

The systemd directory defines boot ordering and service dependencies.

The config directory contains safe examples only. Real credentials never belong in Git.

The deploy directory contains configuration for external services.

The scripts directory contains installer, image builder and diagnostic tooling.

Tests should cover configuration, the boot state machine, network decisions, storage detection and service failures.

Optional future modules may add additional inference engines, a dashboard, agent orchestration, RAG, MCP, hardware telemetry and remote administration without coupling them to the boot controller.

