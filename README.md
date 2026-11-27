# esphome-esp32-platform

## Purpose
ESPHome's `esp32` and `psram` platform components: settings from the detected chip revision and flash chip (one
firmware for several board revisions), crash dumps to flash, chip / flash / PSRAM facts for other components, ESP32-P4
PSRAM speeds up to 250 MHz, fixes for the ESP32-P4 restart and ESP-IDF 6.1, and helpers other topics build on
(replacing an IDF component, the loop task's core, NVS defaults at build time).

## How it works
The `esp32` and `psram` components replace upstream's at build time through `external_components`. A build service
can pass the detected chip revision and flash chip as build variables (`ESPHOME_CHIP_REVISION`, `ESPHOME_FLASH_ID`);
the build then picks the settings that fit, a configured value always wins. Other topic repos use the helpers
(`add_idf_component(override_path=...)`, `LOOP_TASK_CORE`, `nvs_ready`, `system_info.h`), so they take `esp32` from
here.

## Quick start
Chips: ESP32, ESP32-S2, ESP32-S3, ESP32-P4, ESP32-S31

Needs: upstream ESPHome only

```yaml
external_components:
  - source: github://p1ngb4ck/esphome-esp32-platform@main
    components: [esp32, psram]
```

Your own `esp32:` / `psram:` blocks then take the new options, e.g. `framework: advanced: minimum_chip_revision: "3.0"`,
`boot_ram_probe`, `components_in_psram`, `psram: speed: 250MHz` (P4 revision 3; P4 speeds 20 / 80 / 200 / 250 MHz,
100 MHz no longer accepted). The API actions (`chip_revision`, `flash_info`, `nvs_list`, `coredump_read`, ...)
need `api:`; on main they stop the code generation for now (`fnv1_hash` is not imported in `esp32/__init__.py`).

## Main options
| Option | What it does | Default |
|---|---|---|
| `esp32` `advanced:` [`minimum_chip_revision`][o-esp32] | P4: `0.0`, `0.1`, `1.0`, `3.0`, `3.1` | detected |
| `esp32` `advanced:` [`boot_ram_probe`][o-esp32] | log the lowest free internal RAM of the first 90 s | `false` |
| `esp32` `advanced:` [`components_in_psram`][o-esp32] | component objects in a PSRAM section | `false` |
| `esp32` [`board: esp32-s31-function-coreboard-1`][o-esp32] | the ESP32-S31 Function CoreBoard 1 | - |
| [build variables][o-build] | `ESPHOME_CHIP_REVISION`, `ESPHOME_FLASH_ID`, `ESPHOME_FLASH_MAX_FREQUENCY` | - |
| [`coredump` partition][o-core] | crash dumps to flash, read with the API actions | - |
| `psram` [`speed`][o-psram] | ESP32-P4: `20MHz`, `80MHz`, `200MHz`, `250MHz` (revision 3) | variant's |

[o-esp32]: CONFIGURATION.md#esp32
[o-build]: CONFIGURATION.md#build-variables
[o-core]: CONFIGURATION.md#crash-dumps
[o-api]: CONFIGURATION.md#api-actions
[o-psram]: CONFIGURATION.md#psram
[o-auth]: CONFIGURATION.md#for-component-authors

## Catalog
Components (folder on `main` - options):
- [`esp32`](https://github.com/p1ngb4ck/esphome-esp32-platform/tree/main/esphome/components/esp32) - [esp32][o-esp32],
  [build variables][o-build], [crash dumps][o-core], [API actions][o-api], [for component authors][o-auth]
- [`psram`](https://github.com/p1ngb4ck/esphome-esp32-platform/tree/main/esphome/components/psram) - [psram][o-psram]

Branches:
- [main][b-main] (the whole topic)
- [pr/esp32-board-s31-function-coreboard-1][b-1]
- [pr/esp32-boot-ram-probe][b-2]
- [pr/esp32-cert-bundle-sha512][b-3]
- [pr/esp32-chip-revision][b-4]
- [pr/esp32-components-in-psram][b-5]
- [pr/esp32-coredump][b-6]
- [pr/esp32-flash-detect][b-7]
- [pr/esp32-idf-component-override-path][b-8]
- [pr/esp32-idf61-hosted-sdkconfig][b-9]
- [pr/esp32-loop-task-core][b-10]
- [pr/esp32-p4-restart-fix][b-11]
- [pr/esp32-p4-small-alloc-retention][b-12]
- [pr/esp32-system-info][b-13]
- [pr/esp32-warn-once][b-14]
- [pr/psram-ext-stack-guaranteed][b-15]
- [pr/psram-p4-speeds][b-16]

[b-main]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/main
[b-1]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-board-s31-function-coreboard-1
[b-2]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-boot-ram-probe
[b-3]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-cert-bundle-sha512
[b-4]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-chip-revision
[b-5]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-components-in-psram
[b-6]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-coredump
[b-7]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-flash-detect
[b-8]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-idf-component-override-path
[b-9]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-idf61-hosted-sdkconfig
[b-10]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-loop-task-core
[b-11]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-p4-restart-fix
[b-12]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-p4-small-alloc-retention
[b-13]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-system-info
[b-14]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/esp32-warn-once
[b-15]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/psram-ext-stack-guaranteed
[b-16]: https://github.com/p1ngb4ck/esphome-esp32-platform/tree/pr/psram-p4-speeds

Needed topic repos: none (upstream ESPHome only). The NVS branches of `esp32` (`pr/esp32-nvs-ready`,
`pr/esp32-nvs-defaults`, `pr/esp32-nvs-actions`) are in
[esphome-storage-drivers](https://github.com/p1ngb4ck/esphome-storage-drivers).

All topics together: `github://p1ngb4ck/esphome@dev`.

---

# Detailed version

Changes to ESPHome's `esp32` and `psram` platform components: settings from the detected chip revision and flash
chip (one firmware for different board revisions), crash dumps to flash with API access, NVS browsing and erasing
over the API, chip / flash / PSRAM facts for other components, ESP32-P4 PSRAM speeds up to 250 MHz, the ESP32-S31
Function CoreBoard, fixes for the ESP32-P4 restart and ESP-IDF 6.1, and helpers other topics build on (replacing an
IDF component, the loop task's core, NVS defaults at build time).

Base: ESPHome `dev` at `a5a75738ad`.

## Examples

```yaml
external_components:
  - source: github://p1ngb4ck/esphome-esp32-platform@main
    components: [esp32, psram]

esp32:
  variant: esp32p4
  framework:
    type: esp-idf
    advanced:
      minimum_chip_revision: "3.0"

psram:
  speed: 250MHz
```

The ESP32-P4 PSRAM speeds of this repository are 20 / 80 / 200 / 250 MHz: upstream's `speed: 100MHz` on the P4 is
not accepted here (a config with it fails validation).

With `api:` the device answers the API actions `chip_revision`, `flash_info`, `flash_verify`, `nvs_list`,
`nvs_erase` and, with a `coredump` partition in the partition table, `coredump_info`, `coredump_read`,
`coredump_erase`. On `main` these actions currently stop the code generation (`fnv1_hash` is used in
`esp32/__init__.py` but not imported); a config with `api:` does not build until that is fixed.

## More

**[CONFIGURATION.md](CONFIGURATION.md)**: every option, the build variables for chip revision and flash detection,
crash dumps, the helpers for component authors.

## Branches

One branch per change; `main` contains every branch. Base: ESPHome `dev` at `a5a75738ad`.
The NVS branches (`pr/esp32-nvs-ready`, `pr/esp32-nvs-defaults`, `pr/esp32-nvs-actions`) are in
[esphome-storage-drivers](https://github.com/p1ngb4ck/esphome-storage-drivers) with the NVS guard (`pr/nvs`); their
code (the NVS actions `nvs_list` / `nvs_erase`, `nvs_ready`, `add_nvs_default`) is also on this repository's `main`.

Per branch: change; chips; needs.

- `pr/esp32-warn-once`: config-time warnings once per run; all; needs -.
- `pr/esp32-idf-component-override-path`: `add_idf_component(override_path=...)`: a built-in IDF component replaced
  by a project copy; all; needs -.
- `pr/esp32-loop-task-core`: `LOOP_TASK_CORE` in `esp32/hal.h`, the loop task pinned to it; all; needs -.
- `pr/esp32-chip-revision`: ESP32-P4 `minimum_chip_revision`, settings from the detected chip revision, API action
  `chip_revision`; ESP32, P4; needs `pr/esp32-warn-once`.
- `pr/esp32-flash-detect`: flash chip and speed from the detected flash, API actions `flash_info` / `flash_verify`;
  all, speed: P4; needs `pr/esp32-chip-revision`.
- `pr/esp32-coredump`: crash dumps to a `coredump` partition, API actions `coredump_info` / `coredump_read` /
  `coredump_erase`; all; needs `pr/esp32-chip-revision`.
- `pr/esp32-system-info`: `system_info.h`: chip, flash, PSRAM and partition facts, usage per region; all; needs -.
- `pr/esp32-p4-restart-fix`: `esp_restart_noos` wrapped: the other core stalled and the cache sync engine idle before
  the restart (espressif/esp-idf#19171); P4; needs -.
- `pr/esp32-p4-small-alloc-retention`: small malloc / new allocations to the retention RAM; P4; needs -.
- `pr/esp32-boot-ram-probe`: `advanced: boot_ram_probe`: free internal RAM sampled for the first 90 s, logged
  afterwards; all; needs -.
- `pr/esp32-components-in-psram`: `advanced: components_in_psram`: the component objects in a PSRAM section; chips
  with PSRAM; needs -.
- `pr/esp32-board-s31-function-coreboard-1`: board `esp32-s31-function-coreboard-1`; S31; needs -.
- `pr/esp32-idf61-hosted-sdkconfig`: IDF 6.1 Kconfig `default 0` patched; SoftAP / PHY TX power options skipped with
  `esp32_hosted` or IDF 6.1; all; needs -.
- `pr/esp32-cert-bundle-sha512`: the certificate bundle keeps SHA-384 / SHA-512; all; needs -.
- `pr/psram-p4-speeds`: ESP32-P4 PSRAM speeds 20 / 80 / 200 / 250 MHz (upstream's 100 MHz dropped), 250 MHz only on
  revision 3; P4; needs -.
- `pr/psram-ext-stack-guaranteed`: task stacks in PSRAM allowed only when PSRAM is guaranteed; chips with PSRAM;
  needs -.

Single changes can be used on their own: `github://p1ngb4ck/esphome-esp32-platform@pr/esp32-coredump`. All topics
together: `github://p1ngb4ck/esphome@dev`. `external_components` replaces components only; the ESPHome core of the
build host stays as installed.

## License

- ESPHome's own files, also where this repository changes them: the [ESPHome License](LICENSE) (MIT for the Python
  code and everything else, GPLv3 for the C++ runtime files).
- New files written for this repository (new components, their code, tests and documentation): MIT,
  [LICENSE-MIT](LICENSE-MIT).

## Thanks

Everyone this project builds on: [ATTRIBUTIONS.md](https://github.com/p1ngb4ck/esphome/blob/dev/ATTRIBUTIONS.md).

## Branch structure

Each `pr/` branch holds one part of this topic on top of upstream and says in its `BRANCH.md` what it is based on.
`main` holds all of them.

| Branch | Based on |
|---|---|
| `pr/esp32-board-s31-function-coreboard-1` | upstream only |
| `pr/esp32-boot-ram-probe` | upstream only |
| `pr/esp32-cert-bundle-sha512` | upstream only |
| `pr/esp32-chip-revision` | `pr/esp32-warn-once` |
| `pr/esp32-components-in-psram` | upstream only |
| `pr/esp32-coredump` | `pr/esp32-chip-revision` |
| `pr/esp32-flash-detect` | `pr/esp32-chip-revision` |
| `pr/esp32-idf-component-override-path` | upstream only |
| `pr/esp32-idf61-hosted-sdkconfig` | upstream only |
| `pr/esp32-loop-task-core` | upstream only |
| `pr/esp32-p4-restart-fix` | upstream only |
| `pr/esp32-p4-small-alloc-retention` | upstream only |
| `pr/esp32-system-info` | upstream only |
| `pr/esp32-warn-once` | upstream only |
| `pr/psram-ext-stack-guaranteed` | upstream only |
| `pr/psram-p4-speeds` | upstream only |
