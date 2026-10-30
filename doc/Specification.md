# RVSoC9108 Specification

First tapeout of [Vernii](https://github.com/friscv/vernii), a small Linux-capable RISC-V SoC for low-cost devices, with the [FRISC-V](https://github.com/friscv/friscv) core. Pins, clocks, boot and the memory map are in the [datasheet](Datasheet.md).

## Design

The core and the debug module share a memory hub. It sends memory accesses through the cache to the HyperBus controller and register accesses to the peripherals. The cache is 8 KiB, 4-way and write-through, with 64-byte lines. Any of its ways can also serve as on-chip memory at `0x0`, where the boot ROM loads the first stage.

The HyperBus controller is PULP's, with four patches in `hw/vendored/patches` of the source repository. Two 16-tap delay line macros shift `HB_CK` and the incoming `RWDS` by about 90°, which centers them on the data. Their taps are set in the controller's registers and reset to 9 for transmit and 12 for receive.

Clocks:

- `CLK` - everything, no PLL. The HyperBus clock is `CLK`.
- `TCK` - JTAG, crosses into `CLK` in the debug module.
- `RWDS` - HyperBus read data, crosses into `CLK` through a gray FIFO.

`RST#` and `TRST#` are synchronized on chip.

## Sources

Chip repository: [friscv-ihp-sg13cmos5l](https://github.com/friscv/friscv-ihp-sg13cmos5l), tag `v1.0.0`. The rest comes in through Bender, with exact commits in `Bender.lock`.

| Repository | Version | Used for |
| ---------- | ------- | -------- |
| [friscv/vernii](https://github.com/friscv/vernii) | 1.0.3 | SoC |
| [friscv/friscv](https://github.com/friscv/friscv) | 3.0.1 | Core |
| [friscv/friscv-mem-utils](https://github.com/friscv/friscv-mem-utils) | 4.2.0 | Memory hub, cache |
| [friscv/riscv-frac-aclint](https://github.com/friscv/riscv-frac-aclint) | 0.1.3 | ACLINT |
| [friscv/reg-rom](https://github.com/friscv/reg-rom) | 0.1.2 | Boot ROM |
| [pulp-platform/hyperbus](https://github.com/pulp-platform/hyperbus) | `e21a9ce9` | HyperBus, vendored |
| [pulp-platform/riscv-dbg](https://github.com/pulp-platform/riscv-dbg) | 0.10.1 | Debug module |
| [pulp-platform/opentitan_peripherals](https://github.com/pulp-platform/opentitan_peripherals) | 0.4.0 | GPIO, and the QSPI host vendored in Vernii |
| [IHP-GmbH/IHP-Open-PDK](https://github.com/IHP-GmbH/IHP-Open-PDK) | `7ef70c7e` | Cells, SRAMs, DRC and LVS decks |

UART, PLIC and the bus IP are from other PULP repositories.

## Implementation

LibreLane, every tool pinned in `flake.lock`. `make librelane` rebuilds the release, see [RVSoC9108-main](../RVSoC9108-main/README.md).

| Parameter | Value |
| --------- | ----- |
| Die | 2100 µm × 2100 µm, 64 pads |
| Core area | 1428 µm × 1428 µm |
| Standard cells | 1.164 mm², 72 % of the core area outside the macros |
| Clock | 13.33 ns (75 MHz), `TCK` 100 ns |
| Corners | fast, typical, slow |
| Macros | 4 × SRAM 512 × 32, 1 × SRAM 64 × 64, 2 × delay line |

## Signoff

| Check | Tool | Result |
| ----- | ---- | ------ |
| DRC, main deck | KLayout 0.30.9 | Clean |
| DRC, precheck rules | KLayout 0.30.9 | Clean |
| Antenna | KLayout 0.30.9 | Clean |
| LVS | KLayout 0.30.9 | Match, with the PDK's I/O and SRAM cells as black boxes |
| LVS in the flow | Netgen 1.5.320 | Match |
| Setup and hold, 3 corners | OpenSTA | No violations, worst slack 0.060 ns setup, 0.045 ns hold |
| Max slew | OpenSTA | Over the limit only at the pads of 39 I/O signals, accepted |

Details in [RVSoC9108-main](../RVSoC9108-main/README.md#signoff).

## Verification

Simulation is in `target/sim` of the source repository. The C++ harness models two HyperRAMs (with timing checks), the flash, an SD card, the UART and JTAG. The DUT is the RTL of the whole chip with the PDK's pad, SRAM and cell models.

| Simulator | Tests | Result |
| --------- | ----- | ------ |
| Verilator | 8 directed, seeds 1-3 | 24/24 pass |
| Verilator | riscv-arch-test `5f69fff1` | 120/120 pass |

Directed tests cover the cache, HyperRAM, GPIO and SD card, and boot over UART and from flash. Per-test results are in [`sim_results.txt`](../RVSoC9108-main/verification/sim_results.txt).

The final netlist was also simulated at gate level with `make run-gls`.
