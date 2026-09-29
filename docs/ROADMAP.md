# Roadmap

Build reliability first, feature breadth second.

## Phase 0 — Architecture

- system boundary;
- boot state machine;
- storage model;
- network schema;
- dependency graph;
- LAN API boundary.

## Phase 1 — Bootable Debian appliance

Outcome:

~~~text
USB
 ↓
Debian
 ↓
AIBootBox preflight
 ↓
network
 ↓
storage
 ↓
status
~~~

Tasks:

- installer guidance;
- UEFI bootloader on USB;
- systemd base services;
- configuration partition detection;
- structured logs;
- recovery/debug mode.

## Phase 2 — Network controller

- Ethernet detection;
- Wi-Fi scan;
- configured SSID/PSK;
- interactive setup;
- retry logic;
- DNS and route checks;
- LAN address reporting.

Acceptance: the appliance boots either from a configuration file or an interactive local Wi-Fi flow.

## Phase 3 — NVIDIA platform

- driver detection;
- CUDA capability detection;
- GPU health probe;
- version reporting;
- container GPU test;
- diagnostics.

Acceptance: the appliance can clearly determine whether NVIDIA acceleration is usable.

## Phase 4 — Core AI stack

- Ollama;
- model storage;
- Open WebUI;
- automatic startup;
- health aggregation.

Acceptance: boot reaches the web UI and an installed local model can be used.

## Phase 5 — LAN API gateway

- LiteLLM;
- OpenAI-compatible endpoint;
- API keys;
- logical model names;
- firewall;
- LAN documentation.

Acceptance: a remote OpenAI SDK client can call a selected local model through the authenticated gateway.

## Phase 6 — AIBootBox dashboard

Optional custom dashboard showing:

- boot state;
- network;
- IP;
- GPU;
- VRAM;
- RAM;
- storage;
- service health;
- models;
- active model;
- API endpoint.

The dashboard should complement Open WebUI rather than duplicate its chat functionality.

## Phase 7 — Multi-backend and multi-agent

Potential work:

- additional inference engines;
- agent orchestration;
- coding tools;
- MCP;
- RAG/vector storage;
- project workspaces;
- workload routing.

## Phase 8 — Image builder and upgrades

- reproducible image builds;
- release artifacts;
- first-boot configuration;
- upgrade tooling;
- rollback/recovery;
- hardware compatibility matrix.

## Production criteria

Do not consider the appliance production-ready until it can:

- boot repeatedly from USB;
- recover from missing network configuration;
- avoid accidental host-disk modification;
- report failures clearly;
- start services deterministically;
- expose only intended LAN services;
- preserve user models across OS updates;
- reinstall the OS without requiring model re-downloads when DATA is retained.

