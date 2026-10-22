---
title:        README
subtitle:     ''
author:       Getreu
date:         2026-10-18
lang:         en-US
---

# Kanata keyboard definition

[Kanata] is a cross-platform software keyboard remapper for Linux, macOS and
Windows. The [Kanata] configuration file `./kanata-musclememory.kbd`
implements the ["muscle memory friendly" keyboard layout] on regular keyboards.

[Kanata]: https://github.com/jtroo/kanata
["muscle memory friendly" keyboard layout]: https://blog.getreu.net/20250826-muscle-memory-friendly-home-row-mods/

## Right thumb key (`ralt`)

The right thumb key carries three distinct gestures:

| gesture                     | result                              |
|-----------------------------|-------------------------------------|
| tap                         | `Backspace`                         |
| tap, then re-press and hold | `Backspace` repeats                 |
| hold                        | navigation layer, while held        |

The repeat gesture is governed by `$repress-time` (`450`), the first
parameter of every plain `tap-hold`. Kanata measures that window from the
*initial* press rather than from the release, so the tap's own duration eats
into it: after a 60 ms tap there is roughly 340 ms left to re-press. Within
the window the tap action is held down and the OS repeats it; past it, the
key resolves to its hold action instead.

`ralt` keeps all three gestures while also taking part in the `lalt`+`ralt`
chord that switches to the flat symbol layer. Being a chord participant
delays its first press by the per-chord timeout, which is why every chord
whose keys carry a tap-hold uses a short one (`80`): an incomplete chord then
falls through to the active layer quickly, and the hold still arrives
promptly.

`chords-v2-min-idle 200` skips chord processing for 200 ms after any
non-chord activation. Its purpose is ergonomic — it keeps every chord a
deliberate gesture made from a short pause, rather than something that can
trigger mid-typing.

It does **not** preserve a participating key's tap-repress context, which an
earlier version of this README claimed. Measured with
[`kanata_simulated_input`], `Backspace` repeat on `ralt` reaches the same
~340 ms gap at `chords-v2-min-idle 5` as it does at `200`. Raising it to
`450` widens the window to ~420 ms, but only for the chord participants
(`esc`, `lalt`, `ralt`, `spc`), and it delays every chord.

Tap-then-hold repeat is available only on the plain `tap-hold` keys. The home
row mods use `tap-hold-release-keys` with a `(multi … @tap)` tap action,
which never key-repeats at any timeout value.

[`kanata_simulated_input`]: https://github.com/jtroo/kanata/tree/main/simulated_input
