# Network and Wi-Fi

## Goals

AIBootBox supports Ethernet, preconfigured Wi-Fi, interactive Wi-Fi setup, LAN API access and optional internet access for downloads and updates.

NetworkManager is the first implementation layer because it provides persistent connection management and nmcli for command-line control.

## Decision flow

~~~text
              START
                │
                ▼
        Ethernet usable?
          /           \
        YES            NO
         │              │
         ▼              ▼
      ONLINE       config exists?
                       /       \
                     YES        NO
                      │          │
                      ▼          ▼
                connect config  interactive
                      │          │
                      └────┬─────┘
                           ▼
                     verify network
                           │
                     ┌─────┴─────┐
                     ▼           ▼
                  success      failure
                     │           │
                     ▼           ▼
                start stack    retry/debug
~~~

## Configuration partition

The USB should contain a small FAT32 partition named AIBOX-CONFIG so users can edit setup files from Windows, macOS or Linux without mounting the Debian root filesystem.

Example:

~~~text
AIBOX-CONFIG/
├── wifi.yaml
└── aibox.yaml
~~~

The MVP permits plaintext Wi-Fi credentials on that partition. This is convenient but means possession of the USB can expose the password. Encryption/enrollment can be added later.

## Wi-Fi schema

The configuration format should support:

- SSID;
- PSK/password;
- hidden network;
- adapter selection;
- DHCP/static IPv4;
- DNS;
- multiple networks with priorities.

## LAN endpoint design

Suggested defaults:

| Port | Service | Default exposure |
|---|---|---|
| 3000 | Open WebUI | LAN |
| 4000 | LiteLLM | LAN |
| 11434 | Ollama | localhost/private |

All ports should be configurable.

## API boundary

Do not expose raw Ollama to the LAN by default.

Preferred:

~~~text
LAN client
   ↓
LiteLLM :4000
   ↓
Ollama :11434
~~~

The gateway can provide authentication, API keys, model permissions, rate limits and logging.

## Internet versus LAN

These are separate capabilities.

- Installed local models can work without internet.
- LAN chat/API can work without internet.
- Model downloads need internet.
- OS updates need internet.

Diagnostics should report them separately.

## Diagnostics

The future network diagnostic command should report:

- interface;
- link state;
- IPv4;
- gateway;
- DNS;
- LAN reachability;
- internet reachability;
- listening ports;
- configuration source.

It must never print a Wi-Fi password.

