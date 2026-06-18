# zmk-config-tbkmini

ZMK firmware for an AliExpress wireless "TBK Mini" split — really a **36-key**
board (3×5 + 3 thumbs per hand) built on a **V&Z hand-wire adapter** + a
**SuperMini nRF52840** controller per half (builds as nice!nano v2, target
`nice_nano//zmk`). Keymap is adapted from `zmk-config-sweep-pro`.

The matrix/transform was traced by multimeter and then cross-checked against the
seller's own source ([Vzhao-L/zmk-for-charybdis], branch `charybdis-Nano35-MOD`),
which confirmed every pin. The trackball / encoder / RGB hardware in the seller's
Charybdis fork is **not** present on this unit, so none of it is included here.

[Vzhao-L/zmk-for-charybdis]: https://github.com/Vzhao-L/zmk-for-charybdis/tree/charybdis-Nano35-MOD

## Matrix wiring

| Adapter | SuperMini | | Adapter | SuperMini |
|---------|-----------|-|---------|-----------|
| R1      | P0.31     | | C1      | P0.02     |
| R2      | P1.15     | | C2      | P0.29     |
| R3      | P0.24     | | C3      | P0.09     |
| R4      | P0.22     | | C4      | P1.00     |
|         |           | | C5      | P0.11     |

Diode direction: **row2col**. Left half = central, right = peripheral
(`col-offset = 5`). Unused: R5, C6.

## Before you flash anything: back up the stock firmware

1. Double-tap reset on a half → it mounts as a USB drive.
2. Copy **`CURRENT.UF2`** off it (e.g. `stock_left.uf2`); repeat for the other half.
3. To restore later, double-tap reset and drag that file back on.

## Build & flash

1. Push to GitHub (see below) → GitHub Actions builds it.
2. Download `tbkmini_left.uf2` and `tbkmini_right.uf2` from the Actions run.
3. Flash each half (double-tap reset, drag the matching `.uf2` on), then pair.
4. If you ever need to wipe Bluetooth bonds when re-pairing, flash
   `settings_reset.uf2` to a half.

If a key or two is off, it's almost certainly a thumb assignment — tell me what's
wrong and it's a one-line fix. (A whole half being dead would mean diode
direction, but the seller's source confirms row2col, so that's settled.)

## Keymap

Six layers ported from sweep-pro: ABC / NUM / SYM / NAV / FUN / UTIL, with home-row
mods, the caps-word / del / esc / nav-lock combos, and the Bluetooth profile
behaviors. The thumb row is the only structural change — 3 thumbs per hand. Full
thumb row, left→right: **LSHFT TAB ENTER | SPACE BSPC RSHFT**, i.e. the shifts sit
on the outer "tucky" keys (see `config/tbkmini.keymap`).

## Pushing to GitHub (run these yourself)

```sh
cd zmk-config-tbkmini
git init -b main
git add .
git commit -m "Wireless V&Z/SuperMini 36-key TBK Mini: shield + sweep-pro keymap"
gh repo create jusdisgi/zmk-config-tbkmini --public \
  --description "ZMK for the wireless V&Z/SuperMini 36-key TBK Mini"
git remote add origin https://github.com/jusdisgi/zmk-config-tbkmini
git push -u origin main
```

(If `gh repo create` already configured the remote, skip `git remote add`.)
