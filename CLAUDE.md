# CLAUDE.md — zmk-config-tbkmini

Notes for future sessions. Cross-cutting workspace facts live in the top-level
`../CLAUDE.md`; this file is only what's specific to this board.

## What it is

- AliExpress wireless "TBK Mini" — actually a **36-key** board: 3×5 + 3 thumbs
  per hand. NOT the real (wired/QMK, 3×6+3) Bastard Keyboards TBK Mini.
- Per half: **V&Z hand-wire adapter** + **SuperMini nRF52840** (builds as
  `nice_nano_v2`). Left = central, right = peripheral.
- Keymap is derived from `../zmk-config-sweep-pro` (Hunter's reference keymap),
  but with 3 real thumb keys per hand (sweep has 2 + an encoder). The extra,
  **innermost** thumb key per hand is wired as LSHFT (left) / RSHFT (right) —
  the "tucky" key that distinguishes this board from sweep.

## Matrix (measured, both halves identical)

- Rows: R1=P0.31, R2=P1.15, R3=P0.24, R4=P0.22
- Cols: C1=P0.02, C2=P0.29, C3=P0.09, C4=P1.00, C5=P0.11
- diode-direction: col2row (assumed; confirm via matrix test). Unused: R5, C6.

## Gotchas

- The V&Z adapter has **no public pinout** — the table above came from
  continuity tracing. If a half goes dead after a controller swap, re-trace.
- Physical (row,col)→key layout is being established via the matrix-test keymap
  in `config/tbkmini.keymap`. Replace it with the real transform + keymap once
  the test output is in.
- ZMK pinned to the same revision as sweep-pro (`cb615aee…`) for a known-good
  toolchain. The split column offset is handled by `col-offset = <5>` in
  `tbkmini_right.overlay`, same pattern as sweep.
