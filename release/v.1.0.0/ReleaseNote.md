# RVSoC9108 v.1.0.0

First release, for the November 2026 IHP Open Silicon MPW. Built from [friscv-ihp-sg13cmos5l](https://github.com/friscv/friscv-ihp-sg13cmos5l) `v1.0.0` (`cd5d99f`), see [RVSoC9108-main](../../RVSoC9108-main/README.md).

| File | SHA-256 |
| ---- | ------- |
| `gds/RVSoC9108.gds.gz` | `63ba885c69be39ea547af10cd753532d00fbe933971e417074c53a87503999d8` |
| `RVSoC9108.gds`, uncompressed | `6e011da499716be0e5a6e237c2b67b1e2637f8a3320211410a56599ca011886c` |
| `netlist/RVSoC9108.cdl` | `ecc069cef931c6c80c1606aa728b01afc3008bc12542c4e6c5a43fef537c5dc7` |

| Check | Result |
| ----- | ------ |
| DRC, main deck | Clean |
| DRC, precheck rules | Clean |
| Antenna | Clean |
| LVS | Match, with the PDK's I/O and SRAM cells as black boxes |
| Setup and hold, 3 corners | No violations |
| Max slew | Over the limit only at the pads of 39 I/O signals, accepted |
