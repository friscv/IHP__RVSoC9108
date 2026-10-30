# RVSoC9108-main

The release was built from [friscv-ihp-sg13cmos5l](https://github.com/friscv/friscv-ihp-sg13cmos5l) tag `v1.0.0` (`cd5d99ff0136b594f6d3f4416e8580d9b7f5e84f`). RTL dependencies are pinned in `Bender.lock`, the tools in `flake.lock`, and IHP-Open-PDK `7ef70c7e` in `flake.nix`.

## Building

```bash
git clone --branch v1.0.0 https://github.com/friscv/friscv-ihp-sg13cmos5l.git
cd friscv-ihp-sg13cmos5l
nix develop --accept-flake-config
make librelane                          # RTL to GDS, writes target/ihp-sg13cmos5l/release/
make -C target/ihp-sg13cmos5l signoff   # slew check, DRC and LVS on the release
```

This reproduces the CDL exactly, and the GDS except for the timestamps in its headers. The STA and Netgen results in `verification/` come from that rebuild.

## Contents

- `constraints/sdc/` - timing constraints
- `Flow/scripts/` - LibreLane config and `target/ihp-sg13cmos5l/Makefile`, copied from the source repository
- `verification/` - signoff results for the released GDS and CDL. `checked_files.sha256` has their hashes, with paths from the build machine.

RTL, testbenches and flow outputs are in the source repository: `hw/` and `target/ihp-sg13cmos5l/src/` for the RTL, `target/sim/` for the testbenches, `target/ihp-sg13cmos5l/librelane/runs/<run>/` for synthesis and place and route.

## Signoff

| Check | Run on | Result | Files in `verification/` |
| ----- | ------ | ------ | ------------------------ |
| DRC, main deck | rebuild, in LibreLane | Clean | `drc/main_deck/` |
| DRC, precheck rules | released GDS | Clean | `drc/precheck/` |
| Antenna | released GDS | Clean | `drc/antenna/` |
| LVS | released GDS and CDL | Match | `lvs/patched_deck/` |
| LVS, Netgen | rebuild, in LibreLane | Match | `lvs/netgen/` |
| STA, 3 corners | rebuild, in LibreLane | No setup or hold violations | `sta/` |

DRC and LVS are run with `make drc` and `make lvs`, see `Flow/scripts/Makefile`.

The PDK's I/O pads and SRAMs are black boxes in LVS (`sg13cmos5l.lvs.patch`). The stock deck also checks their insides, and those don't match IHP's own CDL, see `lvs/stock_deck/`. The released CDL has the PDK's I/O cell CDL with `sg13cmos5l_io.cdl.patch` applied.

Slew is over the limit on 39 pads because of the board load and input transition in the SDC. We accepted these.
