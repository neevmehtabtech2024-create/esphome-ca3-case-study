# 1. Project and Repository Investigation

## 1.1 What is ESPHome?
ESPHome is an open-source system for configuring and controlling microcontrollers such as ESP32 and ESP8266 using YAML configuration files. It generates firmware/configuration for supported devices and integrates them with home-automation systems. Its core repository contains the configuration parser, component implementations, tests, and development tooling.

Official sources:
- Repository: https://github.com/esphome/esphome
- Product documentation: https://esphome.io/
- Developer documentation: https://developers.esphome.io/

## 1.2 Why this is a suitable open-source case
The selected case concerns a real regression reported by a user, linked to a fix in the upstream repository, and accompanied by a regression test. It allows investigation of issue reporting, reproduction, Python/YAML configuration processing, test fixtures, code review, CI, and release inclusion without requiring a physical microcontroller for the configuration-parser example.

## 1.3 Repository areas relevant to this issue
The PR identifies these exact paths:
- `esphome/components/substitutions/__init__.py` — substitution resolution implementation changed by the fix.
- `tests/unit_tests/fixtures/substitutions/18-dict_self_reference.input.yaml` — regression-test input fixture added by the PR.
- `tests/unit_tests/fixtures/substitutions/18-dict_self_reference.approved.yaml` — expected output fixture added by the PR.

The project is larger than these files. These are the directly relevant paths identified in PR #16273.

## 1.4 How to investigate a repository
1. Read the root README and contribution guide.
2. Open the issue and record expected versus observed behaviour.
3. Open the linked PR and inspect its description, commit, changed files, test fixture, review, and checks.
4. Compare the input fixture with the approved output fixture.
5. Run the test only after setting up the correct project environment; record exact commands and results.
6. Separate upstream evidence from your group's own experiment.

## 1.5 Findings
The issue is not about a physical sensor or board failure. It is a configuration-processing regression: a nested substitution inside a dictionary-valued substitution was not resolved correctly. The example uses `device.manufacturer`, `device.model`, and `device.mac_suffix` to construct `device.name`, which is then used by `esphome.name`.
