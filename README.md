# BOSH Lemon Release

> Ensure your release makes lemonade.

The BOSH Lemon Release provides a way to introduce flakiness for BOSH
deployments. The idea is to use this tool to help build more robustness into
the various components in BOSH and releases. It currently supports:

- Network flakiness, via [comcast](https://github.com/tylertreat/comcast)

## Example: Network Flakiness in a Deployment

```yaml
---
name: critical-deployment

releases:
  - name: critical-release
    url: <url>
  - name: bosh-lemon-release
    url: <url>

jobs:
  - name: critical-job
    templates:
    - name: critical_daemon
      release: critical-release
    - name: flaky_network
      release: bosh-lemon-release

    properties:
      flaky_network:
        packet_loss: 30%
        target_addr: 8.8.8.8,10.0.0.0/24  # google DNS, a private network
        target_port: 22,53,80,443         # ssh, dns, http, https
```

## Limitations

- Suitable for deployment only to Linux stemcells, due to the fact that we have
  only built the `comcast` binary for Linux.
