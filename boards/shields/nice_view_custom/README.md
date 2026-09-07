# nice!view (custom)

A local fork of ZMK's built-in `nice_view` shield, vendored into this repo
so the top-right box of the central (left) display shows **currently held
modifiers** (Ctrl / Shift / Alt / Gui) instead of the WPM graph.

Everything else (battery, output/BLE status, profile circles, layer name,
and the peripheral-side battery/art screen) is unchanged from upstream.

## How it works

`widgets/status.c` polls `zmk_hid_get_explicit_mods()` every 100 ms (there
is no dedicated "modifiers changed" event wired up in ZMK yet, and
mod-tap/sticky-key/mod-morph behaviors update HID modifier state without
raising `zmk_keycode_state_changed` for a modifier keycode, so listening
for that alone would miss them). The display only redraws when the
modifier bitmask actually changes.

Active modifiers are drawn as filled tiles (inverted colors), inactive
ones as outlined tiles, in a 2x2 grid: CTRL / SHFT on top, ALT / GUI on
the bottom.

## Using it

`build.yaml` builds `nice_view_custom` instead of `nice_view` for both
halves. To go back to the stock WPM widget, swap `nice_view_custom` back
to `nice_view` in `build.yaml`.
