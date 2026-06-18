# zmk-config-tbkmini

ZMK firmware for an AliExpress wireless "TBK Mini" split — really a **36-key**
board (3×5 + 3 thumbs per hand) built on a **V&Z hand-wire adapter** + a
**SuperMini nRF52840** controller per half (builds as nice!nano v2). Keymap is
adapted from `zmk-config-sweep-pro`.

> **Status: bring-up.** The active keymap is a temporary *matrix test* used to
> learn the physical layout. See "Bring-up" below.

## Measured matrix wiring

Traced with a multimeter from the V&Z adapter pads to the SuperMini pins (both
halves use the same adapter, so the mapping is identical):

| Adapter | SuperMini | | Adapter | SuperMini |
|---------|-----------|-|---------|-----------|
| R1      | P0.31     | | C1      | P0.02     |
| R2      | P1.15     | | C2      | P0.29     |
| R3      | P0.24     | | C3      | P0.09     |
| R4      | P0.22     | | C4      | P1.00     |
|         |           | | C5      | P0.11     |

Diode direction is assumed **col2row** (matches sweep-pro). Unused: R5, C6.

## Before you flash anything: back up the stock firmware

The board ships with working firmware. Save it as a safety net so you can always
return to a known-good state:

1. Double-tap the reset button on a half → it mounts as a USB drive.
2. Copy **`CURRENT.UF2`** off it to your computer (e.g. `stock_left.uf2`).
3. Repeat for the other half (`stock_right.uf2`).

To restore later, double-tap reset and drag that file back on.

## Bring-up (do this first)

The matrix wiring is known, but *which physical key sits at each (row,col)* is
not — so the first build is a **matrix test**. Each key types a unique
character; pressing every key once reveals the full layout.

1. Push this repo to GitHub (see below) → GitHub Actions builds it.
2. Download `tbkmini_left.uf2` and `tbkmini_right.uf2` from the Actions run.
3. Flash each half (double-tap reset, drag the matching `.uf2` on).
4. Pair the halves, open a text editor, and press **every key once** in a tidy
   pass (top row left→right, each row down, then thumbs).
5. Paste the output back to me. I'll generate the final transform + your
   sweep-pro keymap (with the innermost tuckies as LSHFT / RSHFT).

The decode table is documented at the top of `config/tbkmini.keymap`. If
**nothing** types on either half, the board is row2col — say so and I'll flip
the shield.

## Pushing to GitHub (run these yourself)

```sh
cd zmk-config-tbkmini
git init -b main
git add .
git commit -m "Initial bring-up: shield + matrix test keymap"
gh repo create jusdisgi/zmk-config-tbkmini --public \
  --description "ZMK for the wireless V&Z/SuperMini 38-key TBK Mini"
git remote add origin https://github.com/jusdisgi/zmk-config-tbkmini
git push -u origin main
```

(If `gh repo create` already configured the remote, skip `git remote add`.)
