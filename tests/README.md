# Tests

Test the appliance by layer.

## Unit

Configuration parsing, validation and state-machine transitions.

## Integration

NetworkManager, mount handling, systemd ordering and service health.

## Hardware

NVIDIA, Ethernet-only, Wi-Fi-only, no-GPU and missing-data-drive scenarios.

## Failure tests

The most important tests are recovery tests:

- wrong Wi-Fi password;
- no network;
- missing configuration partition;
- missing data disk;
- failed NVIDIA check;
- failed Ollama;
- failed LiteLLM;
- failed Open WebUI.

A failure should produce a useful diagnostic state rather than a boot loop.

