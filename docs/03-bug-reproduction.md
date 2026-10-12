# 3. Bug Reproduction Plan

**Status:** Not yet run by the student group. Complete the evidence log after running it.

## 3.1 Goal
Compare the same minimal configuration on an affected version and a version containing the fix. This verifies whether the substitution expression is left unresolved on the affected version and resolved on the fixed version.

## 3.2 Prerequisites
- Docker installed and running.
- A terminal in a new, empty working directory.
- Internet access to pull ESPHome images.
- Save the YAML below as `test-device.yaml`.

No physical ESP board is required to test the configuration parser.

## 3.3 Test file
```yaml
substitutions:
  device:
    manufacturer: "espressif"
    model: "esp32"
    mac_suffix: "ffffff"
    name: ${device.manufacturer}-${device.model}-${device.mac_suffix}

esphome:
  name: ${device.name}

esp8266:
  board: esp01_1m
```

This intentionally follows the issue's minimal example. The board declaration is not meant to represent a recommended deployment; the target is substitution parsing.

## 3.4 Test the affected version
Run from the directory containing `test-device.yaml`:

```bash
docker run --rm -v "${PWD}:/config" ghcr.io/esphome/esphome:2026.4.0 config test-device.yaml
```

On Windows PowerShell, `${PWD}` is valid PowerShell interpolation. If using Command Prompt rather than PowerShell, the path syntax differs; use an absolute Windows path in the Docker volume mapping or use PowerShell for this command.

Expected result based on the issue: configuration validation fails with a warning that `device` is undefined while resolving the nested name, and `$` remains in the name. This is the upstream-reported expectation, not yet our result.

## 3.5 Test the fixed version
The PR was included in ESPHome 2026.4.5. Run:

```bash
docker run --rm -v "${PWD}:/config" ghcr.io/esphome/esphome:2026.4.5 config test-device.yaml
```

Expected result based on the fix: substitution resolution succeeds and `device.name` becomes `espressif-esp32-ffffff`. Record the actual exit code and output; do not assume success without running it.

## 3.6 What to capture
1. Screenshot of the YAML file.
2. Screenshot or copied terminal output from 2026.4.0.
3. Screenshot or copied terminal output from 2026.4.5.
4. Exact command, date, OS, Docker version, and image tag.
5. If either command behaves differently, preserve the output and investigate rather than editing the evidence to match expectations.

## 3.7 Evidence log — fill in after testing
| Field | Result |
|---|---|
| Date/time | [Fill in] |
| OS and terminal | [Fill in] |
| Docker version | [Fill in] |
| 2026.4.0 result / exit code | [Fill in; attach genuine log] |
| 2026.4.5 result / exit code | [Fill in; attach genuine log] |
| Difference observed | [Fill in] |
| Problems or deviations | [Fill in] |

## 3.8 If Docker is unavailable
Do not claim reproduction. You can still document the original reporter's reproduction and the upstream fix as secondary investigation, clearly labelling your own reproduction as “not completed.” Alternatively, install Docker Desktop and retry. Do not use current ESPHome only for the affected-version test, because the bug was reported for specific historical versions.

## Source
Issue #16254: https://github.com/esphome/esphome/issues/16254
PR #16273: https://github.com/esphome/esphome/pull/16273
