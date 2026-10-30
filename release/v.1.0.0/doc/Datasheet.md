# RVSoC9108 Datasheet

Linux-capable 32-bit RISC-V SoC in IHP SG13CMOS5L. First tapeout of [Vernii](https://github.com/friscv/vernii) and the [FRISC-V](https://github.com/friscv/friscv) core, built from [friscv-ihp-sg13cmos5l](https://github.com/friscv/friscv-ihp-sg13cmos5l) tag [`v1.0.0`](https://github.com/friscv/friscv-ihp-sg13cmos5l/releases/tag/v1.0.0) (`cd5d99f`).

Preliminary, not yet measured in silicon.

## Features

- FRISC-V RV32IMA core with Zicsr, Zifencei, Zicntr and Sstc
  - five-stage in-order pipeline
  - machine, supervisor and user modes
  - Sv32 MMU with 2 instruction and 4 data TLB entries and a hardware page-table walker
  - iterative multiplier and divider
- 8 KiB 4-way write-through cache, 64-byte lines, any way usable as on-chip memory at `0x0`
- HyperBus controller with two chip selects for 2 × 32 MiB HyperRAM
- QSPI host with three chip selects (boot flash, SD card, spare)
- 16550-compatible UART, 8 GPIOs, ACLINT, PLIC and a system control block
- RISC-V debug module with JTAG
- Boot from QSPI flash, UART or JTAG
- 75 MHz at all signoff corners
- 2.1 mm × 2.1 mm die with 64 pads

## Pinout

Pads are numbered counterclockwise, starting at the north end of the west side. In the Side column, W0 and E0 are at the south end, S0 and N0 at the west end. Signal pads are 3.3 V, outputs drive 30 mA.

Type: I input, O output, IO bidirectional, P supply, G ground.

| Pad | Side | Name | Type | Cell | Description |
| --: | ---- | ---- | ---- | ---- | ----------- |
| 1 | W15 | VDD | P | `sg13cmos5l_IOPadVdd` | Core supply, 1.2 V |
| 2 | W14 | VSS | G | `sg13cmos5l_IOPadVss` | Core ground |
| 3 | W13 | TRST# | I | `sg13cmos5l_IOPadIn` | JTAG reset, active low |
| 4 | W12 | VSS | G | `sg13cmos5l_IOPadVss` | Core ground |
| 5 | W11 | TCK | I | `sg13cmos5l_IOPadIn` | JTAG clock |
| 6 | W10 | VDD | P | `sg13cmos5l_IOPadVdd` | Core supply, 1.2 V |
| 7 | W9 | TMS | I | `sg13cmos5l_IOPadIn` | JTAG mode select |
| 8 | W8 | TDI | I | `sg13cmos5l_IOPadIn` | JTAG data in |
| 9 | W7 | TDO | O | `sg13cmos5l_IOPadOut30mA` | JTAG data out |
| 10 | W6 | IOVSS | G | `sg13cmos5l_IOPadIOVss` | I/O ground |
| 11 | W5 | UART0_TX | O | `sg13cmos5l_IOPadOut30mA` | UART transmit |
| 12 | W4 | UART0_RX | I | `sg13cmos5l_IOPadIn` | UART receive |
| 13 | W3 | RST# | I | `sg13cmos5l_IOPadIn` | Active-low reset |
| 14 | W2 | IOVSS | G | `sg13cmos5l_IOPadIOVss` | I/O ground |
| 15 | W1 | CLK | I | `sg13cmos5l_IOPadIn` | System clock input, also the HyperBus clock |
| 16 | W0 | IOVDD | P | `sg13cmos5l_IOPadIOVdd` | I/O supply, 3.3 V |
| 17 | S0 | HB_DQ0 | IO | `sg13cmos5l_IOPadInOut30mA` | HyperBus data 0 |
| 18 | S1 | HB_DQ1 | IO | `sg13cmos5l_IOPadInOut30mA` | HyperBus data 1 |
| 19 | S2 | HB_DQ2 | IO | `sg13cmos5l_IOPadInOut30mA` | HyperBus data 2 |
| 20 | S3 | HB_DQ3 | IO | `sg13cmos5l_IOPadInOut30mA` | HyperBus data 3 |
| 21 | S4 | IOVSS | G | `sg13cmos5l_IOPadIOVss` | I/O ground |
| 22 | S5 | HB_DQ4 | IO | `sg13cmos5l_IOPadInOut30mA` | HyperBus data 4 |
| 23 | S6 | HB_DQ5 | IO | `sg13cmos5l_IOPadInOut30mA` | HyperBus data 5 |
| 24 | S7 | HB_DQ6 | IO | `sg13cmos5l_IOPadInOut30mA` | HyperBus data 6 |
| 25 | S8 | HB_DQ7 | IO | `sg13cmos5l_IOPadInOut30mA` | HyperBus data 7 |
| 26 | S9 | HB_RWDS | IO | `sg13cmos5l_IOPadInOut30mA` | HyperBus read-write data strobe |
| 27 | S10 | IOVDD | P | `sg13cmos5l_IOPadIOVdd` | I/O supply, 3.3 V |
| 28 | S11 | HB_CK | O | `sg13cmos5l_IOPadOut30mA` | HyperBus clock |
| 29 | S12 | VDD | P | `sg13cmos5l_IOPadVdd` | Core supply, 1.2 V |
| 30 | S13 | HB_CS0# | O | `sg13cmos5l_IOPadOut30mA` | HyperRAM 0 chip select |
| 31 | S14 | HB_CS1# | O | `sg13cmos5l_IOPadOut30mA` | HyperRAM 1 chip select |
| 32 | S15 | VSS | G | `sg13cmos5l_IOPadVss` | Core ground |
| 33 | E0 | HB_RST# | O | `sg13cmos5l_IOPadOut30mA` | HyperRAM reset, active low |
| 34 | E1 | PA0 | IO | `sg13cmos5l_IOPadInOut30mA` | GPIO port A, bit 0 |
| 35 | E2 | PA1 | IO | `sg13cmos5l_IOPadInOut30mA` | GPIO port A, bit 1 |
| 36 | E3 | PA2 | IO | `sg13cmos5l_IOPadInOut30mA` | GPIO port A, bit 2 |
| 37 | E4 | IOVSS | G | `sg13cmos5l_IOPadIOVss` | I/O ground |
| 38 | E5 | IOVDD | P | `sg13cmos5l_IOPadIOVdd` | I/O supply, 3.3 V |
| 39 | E6 | PA3 | IO | `sg13cmos5l_IOPadInOut30mA` | GPIO port A, bit 3 |
| 40 | E7 | PA4 | IO | `sg13cmos5l_IOPadInOut30mA` | GPIO port A, bit 4 |
| 41 | E8 | VSS | G | `sg13cmos5l_IOPadVss` | Core ground |
| 42 | E9 | HRTBT | O | `sg13cmos5l_IOPadOut30mA` | Heartbeat, CLK divided by 2^27 |
| 43 | E10 | VDD | P | `sg13cmos5l_IOPadVdd` | Core supply, 1.2 V |
| 44 | E11 | PA5 | IO | `sg13cmos5l_IOPadInOut30mA` | GPIO port A, bit 5 |
| 45 | E12 | PA6 | IO | `sg13cmos5l_IOPadInOut30mA` | GPIO port A, bit 6 |
| 46 | E13 | PA7 | IO | `sg13cmos5l_IOPadInOut30mA` | GPIO port A, bit 7 |
| 47 | E14 | IOVSS | G | `sg13cmos5l_IOPadIOVss` | I/O ground |
| 48 | E15 | IOVDD | P | `sg13cmos5l_IOPadIOVdd` | I/O supply, 3.3 V |
| 49 | N15 | QSPI0_IO0 | IO | `sg13cmos5l_IOPadInOut30mA` | QSPI data 0 |
| 50 | N14 | QSPI0_IO1 | IO | `sg13cmos5l_IOPadInOut30mA` | QSPI data 1 |
| 51 | N13 | QSPI0_IO2 | IO | `sg13cmos5l_IOPadInOut30mA` | QSPI data 2 |
| 52 | N12 | QSPI0_IO3 | IO | `sg13cmos5l_IOPadInOut30mA` | QSPI data 3 |
| 53 | N11 | VSS | G | `sg13cmos5l_IOPadVss` | Core ground |
| 54 | N10 | VDD | P | `sg13cmos5l_IOPadVdd` | Core supply, 1.2 V |
| 55 | N9 | QSPI0_SCK | O | `sg13cmos5l_IOPadOut30mA` | QSPI clock |
| 56 | N8 | IOVSS | G | `sg13cmos5l_IOPadIOVss` | I/O ground |
| 57 | N7 | IOVDD | P | `sg13cmos5l_IOPadIOVdd` | I/O supply, 3.3 V |
| 58 | N6 | QSPI0_CS0# | O | `sg13cmos5l_IOPadOut30mA` | QSPI chip select 0, boot flash |
| 59 | N5 | QSPI0_CS1# | O | `sg13cmos5l_IOPadOut30mA` | QSPI chip select 1, SD card |
| 60 | N4 | QSPI0_CS2# | O | `sg13cmos5l_IOPadOut30mA` | QSPI chip select 2, spare |
| 61 | N3 | BOOT0 | I | `sg13cmos5l_IOPadIn` | Boot select, bit 0 |
| 62 | N2 | BOOT1 | I | `sg13cmos5l_IOPadIn` | Boot select, bit 1 |
| 63 | N1 | VSS | G | `sg13cmos5l_IOPadVss` | Core ground |
| 64 | N0 | VDD | P | `sg13cmos5l_IOPadVdd` | Core supply, 1.2 V |

## Operating Conditions

Signoff corners, not measured limits:

| Corner | VDD | IOVDD | Temperature |
| ------ | --- | ----- | ----------- |
| Fast | 1.32 V | 3.6 V | −40 °C |
| Typical | 1.20 V | 3.3 V | 25 °C |
| Slow | 1.08 V | 3.0 V | 125 °C |

| Parameter | Value |
| --------- | ----- |
| Clock frequency | up to 75 MHz (13.33 ns period) |
| Core supply, VDD | 1.2 V nominal |
| I/O supply, IOVDD | 3.3 V nominal |

There is no PLL. The HyperBus clock is `CLK`, so keep its duty cycle close to 50 %.

## Reset and Boot

`RST#` resets the chip, `TRST#` the JTAG TAP. Both are active low.

The core starts in the boot ROM at `0x0304_0000`. The ROM reads `BOOT[1:0]` and loads a 4 KiB first stage into on-chip memory at `0x0`:

| BOOT1 | BOOT0 | Source |
| ----- | ----- | ------ |
| 0 | 0 | None, waits for the debugger |
| 0 | 1 | QSPI flash on `QSPI0_CS0#`, from offset 0 |
| 1 | x | UART0 |

Flash is read with SCK at `CLK`/4. UART boot uses divisor 27, which is 115200 baud at 50 MHz. Image layouts are in Vernii's [SOFTWARE_STACK.md](https://github.com/friscv/vernii/blob/main/docs/SOFTWARE_STACK.md).

## Memory Map

| Block | Start | End |
| ----- | ----- | --- |
| On-chip memory (cache ways) | `0x0000_0000` | `0x0000_2000` |
| ACLINT | `0x0200_0000` | `0x0202_0000` |
| System control block | `0x0300_0000` | `0x0300_1000` |
| UART0 | `0x0301_0000` | `0x0301_1000` |
| QSPI0 | `0x0302_0000` | `0x0302_1000` |
| GPIO port A | `0x0303_0000` | `0x0303_1000` |
| Boot ROM | `0x0304_0000` | `0x0304_1000` |
| Debug module | `0x0305_0000` | `0x0305_1000` |
| PLIC | `0x0C00_0000` | `0x0C20_2000` |
| HyperBus configuration | `0x5001_0000` | `0x5001_1000` |
| HyperRAM, cached | `0x8000_0000` | `0x8400_0000` |

`SCB.SYSID` (offset `0x100`) reads `0x5645524E`, "VERN".

## JTAG

RISC-V debug module. IDCODE `0x00000DB3`.

## Physical Data

| Parameter | Value |
| --------- | ----- |
| Process | IHP SG13CMOS5L, 130 nm, 5 metal layers |
| PDK revision | `7ef70c7e871f7c8868bf5ed7488b1837c48fc022` |
| Die and seal ring | 2100 µm × 2100 µm, 4.41 mm² |
| Core area | 1428 µm × 1428 µm |
| Pads | 64, 41 signal and 23 supply |
| Macros | 4 × SRAM 512 × 32 (cache data), 1 × SRAM 64 × 64 (cache tags), 2 × delay line (HyperBus) |
| Standard-cell area | 1.164 mm², 72 % of the core area outside the macros |
| Top cell | `RVSoC9108` |
