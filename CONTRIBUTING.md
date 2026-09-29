# Contributing

AIBootBox is a modular appliance project.

Before implementing a feature:

1. Identify the subsystem that owns it.
2. Define the interface with neighboring subsystems.
3. Update relevant documentation.
4. Add tests for failure cases as well as successful cases.
5. Keep credentials, generated secrets and machine-specific state out of Git.

Prefer small, reviewable changes over one large bootstrap script.

The boot controller should orchestrate Linux services rather than replace them.

