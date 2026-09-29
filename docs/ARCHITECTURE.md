# AIBootBox Architecture

## System boundary

AIBootBox owns appliance lifecycle and infrastructure orchestration. Established open-source projects own specialized functions.

~~~text
                         AIBootBox
┌─────────────────────────────────────────────────────────┐
│ Boot controller                                         │
│  ├─ configuration                                       │
│  ├─ preflight                                           │
│  ├─ network state                                       │
│  ├─ storage state                                       │
│  ├─ GPU health                                          │
│  └─ service orchestration                               │
│                                                         │
│ AI platform                                             │
│  ├─ Open WebUI     → user interface                     │
│  ├─ LiteLLM        → LAN/API gateway                    │
│  ├─ Ollama         → first local inference backend      │
│  └─ future engines → other backends                     │
└─────────────────────────────────────────────────────────┘
~~~

## Runtime planes

### Boot plane

UEFI → Linux → systemd → AIBootBox preflight

### Infrastructure plane

Network → storage → NVIDIA/GPU → Docker

### AI plane

Ollama and future inference engines provide model execution.

### Access plane

Browser users use Open WebUI. Remote programs use the authenticated LiteLLM endpoint.

## Desired dependency graph

~~~text
local-fs.target
      │
      ▼
aibox-boot.service
      │
      ├──────────────┐
      ▼              ▼
network setup     storage setup
      │              │
      └──────┬───────┘
             ▼
       aibox-health
             │
      ┌──────┼─────────┐
      ▼      ▼         ▼
    Docker  Ollama   GPU runtime
      │      │
      │      └─────────────┐
      │                    ▼
      └───────────────► LiteLLM
                            │
                            ▼
                        Open WebUI
~~~

The implementation should use explicit systemd dependency relationships instead of sleep-based startup delays.

## State machine

~~~text
INIT
 │
 ▼
PREFLIGHT
 │
 ├─ failure → DEGRADED
 │
 ▼
NETWORK
 │
 ├─ failure → NETWORK_WAIT / INTERACTIVE
 │
 ▼
STORAGE
 │
 ├─ failure → DEGRADED
 │
 ▼
GPU
 │
 ├─ unavailable → CPU_ONLY or DEGRADED
 │
 ▼
SERVICES
 │
 ├─ partial failure → DEGRADED
 │
 ▼
READY
~~~

The state machine should be visible in diagnostics.

## Failure isolation

- Wrong Wi-Fi password: retry or interactive setup.
- Missing network: AI services remain stopped when network is required.
- Missing data drive: boot to degraded/diagnostic state.
- Missing NVIDIA: mark GPU-dependent capabilities unavailable.
- Ollama failure: report backend failure without corrupting unrelated services.
- LiteLLM failure: local backend may remain available internally.
- Open WebUI failure: API services may remain available.
- Broken model: isolate that model from the rest of the stack.

## Hardware abstraction

Expose capability information such as:

- GPU vendor;
- GPU model;
- VRAM;
- driver status;
- compute/runtime availability.

The initial implementation targets NVIDIA/CUDA but should be extensible to other accelerators later.

## Remote API abstraction

A LAN client should select a logical model name rather than knowing the underlying runtime.

~~~text
OpenAI SDK / coding agent
          │
          ▼
   AIBootBox LAN API
          │
          ▼
       LiteLLM
          │
     ┌────┴─────┐
     ▼          ▼
   Ollama    future backend
~~~

This allows later multi-agent routing without changing every client.

