# CLAUDE.md — zmk-config-tbkmini

Notes for future sessions. Cross-cutting workspace facts live in the top-level
`../CLAUDE.md`; this file is only what's specific to this board.

## What it is

- AliExpress wireless "TBK Mini" — actually a **36-key** board: 3×5 + 3 thumbs
  per hand. NOT the real (wired/QMK, 3×6+3) Bastard Keyboards TBK Mini.
- Per half: **V&Z hand-wire adapter** + **SuperMini nRF52840** (builds as
  `nice_nano_v2`). Left = central, right = peripheral.
- Keymap is derived from `../zmk-config-sweep-pro` (Hunter's reference keymap),
  but with 3 real thumb keys per hand (sweep has 2 + an encoder). Thumb row,
  left→right: LSHFT TAB ENTER | SPACE BSPC RSHFT — shifts on the outer "tucky"
  keys. That extra thumb per hand is what distinguishes this board from sweep.

## Matrix (measured, both halves identical)

- Rows: R1=P0.31, R2=P1.15, R3=P0.24, R4=P0.22
- Cols: C1=P0.02, C2=P0.29, C3=P0.09, C4=P1.00, C5=P0.11
- diode-direction: **row2col**. Unused: R5, C6.

## Gotchas

- The V&Z adapter has **no public pinout** — the table above came from
  continuity tracing, then confirmed pin-for-pin against the seller's source
  ([Vzhao-L/zmk-for-charybdis], `charybdis-Nano35-MOD`). If a half goes dead
  after a controller swap, re-trace.
- The transform (incl. the reversed right-hand columns) is adopted from that
  seller source; it's a Charybdis "Nano35" derivative minus trackball/encoder/RGB.
- ZMK pinned to xiphos's revision (`773dec58…`, newer than sweep-pro's). The
  split column offset is handled by `col-offset = <5>` in `tbkmini_right.overlay`,
  same pattern as sweep.

[Vzhao-L/zmk-for-charybdis]: https://github.com/Vzhao-L/zmk-for-charybdis/tree/charybdis-Nano35-MOD
