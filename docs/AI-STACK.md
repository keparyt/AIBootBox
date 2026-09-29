# AI Stack

## Layering

~~~text
                    User
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
       Browser               API client
          │                     │
          ▼                     ▼
    Open WebUI               LiteLLM
          │                     │
          └──────────┬──────────┘
                     ▼
                AI backends
                     │
                 ┌───┴────┐
                 ▼        ▼
              Ollama   future engines
                 │
                 ▼
               GPU/CPU
~~~

## Open WebUI

Open WebUI is the primary human-facing interface.

Responsibilities include chat, conversations, model selection and supported model-management functions. Its provider-oriented architecture makes it suitable as the UI layer rather than the inference engine.

## Ollama

Ollama is the first local model runtime.

Responsibilities include model storage, model lifecycle and inference.

It should remain a backend service and not become the AIBootBox control plane.

## LiteLLM

LiteLLM is the normal LAN-facing gateway.

Responsibilities:

- OpenAI-compatible API;
- logical model names;
- backend abstraction;
- API keys;
- routing and future fallbacks;
- access control and usage accounting where configured.

## Logical model names

Expose stable names where possible.

~~~text
qwen-coder
    ↓
ollama/qwen...
~~~

Clients should depend on the logical name rather than a fragile local tag.

## Model management

The intended management view is:

~~~text
MODELS
├── Installed
│   ├── name
│   ├── size
│   ├── capability
│   └── status
└── Download
    ├── browse
    ├── search
    ├── pull
    ├── progress
    └── cancel
~~~

AIBootBox should not hard-code a single model catalog. The provider should remain authoritative for installed/downloadable models.

## GPU telemetry

The appliance should expose:

- GPU name;
- driver status;
- VRAM total;
- VRAM used;
- utilization;
- temperature when available.

The first-class acceleration target is NVIDIA/CUDA.

The NVIDIA installation path should separately record driver and CUDA runtime/toolkit versions.

## Resource awareness

The model manager should eventually consider:

- available VRAM;
- available system RAM;
- model size;
- context size;
- backend requirements.

File size alone is not enough to decide whether a model can run.

## Future multi-agent layer

Later releases may add:

- coding agents;
- research agents;
- reviewer agents;
- task orchestration;
- MCP;
- local RAG;
- vector storage;
- workspace management.

These should connect through stable service APIs and remain independent of the boot controller.

