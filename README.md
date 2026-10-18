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

| gesture                      | result                              |
|------------------------------|-------------------------------------|
| tap                          | `Backspace`                         |
| tap, then hold within 200 ms | `Backspace` repeats                 |
| hold                         | navigation layer, while held        |

It keeps all three while also taking part in the `lalt`+`ralt` chord that
switches to the flat symbol layer. That combination needs two timers in
`./kanata-musclememory.kbd`, because `defchordsv2` otherwise suppresses a
participating key's own tap-hold resolution:

- a short per-chord timeout (`80`) on every chord whose keys carry a
  tap-hold, so an incomplete chord falls through to the active layer
  quickly and the hold still arrives promptly;
- `chords-v2-min-idle 200`, which skips chord processing for 200 ms after
  any non-chord activation, so the second press of a tap-then-hold reaches
  the tap-hold with its repress context intact.

The second one is global, so every chord has to be a deliberate gesture made
from a short pause rather than mid-typing.
