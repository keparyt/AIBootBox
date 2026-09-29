# systemd Layout

The systemd directory will contain AIBootBox-owned units.

Planned units:

- aibox-boot.service
- aibox-network.service
- aibox-storage.service
- aibox-health.service
- aibox.target

The units should use explicit dependencies and conditions.

AIBootBox should enable/disable the aggregate target instead of manually starting services in an infinite shell script.

External services such as Docker, Ollama and Open WebUI should be integrated through documented dependency relationships.

