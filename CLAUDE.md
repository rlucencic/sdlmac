# rpi5mac: the Macintosh Plus, SE and Classic on the Raspberry Pi 5

esp32mac's Mac (`../esp32mac/components/macintosh`, platform independent)
on the rpi5emu runtime (`../rpi5emu`, its CLAUDE.md: the workflow, the Pi,
the lessons), the 68000 (Musashi, no options) esp32emu's. The machine's own
behaviour, lessons and tests are esp32mac's CLAUDE.md; this project is the
glue.

## Layout

| Path | What |
|---|---|
| `src/main.c` | The Mac as an `emu_machine_t`: esp32mac's `main/main.c` through `../rpi5emu/tools/port_glue.py` and `port_finish.py`, then by hand: up to 4 MB, no opcode table copies (rpi5ste's lesson), the ROM message, the web script built in, `shutdown()` saves the PRAM |
| `src/settings.c` | esp32mac's, over `emu/nvs.h` (section `mac`, the PRAM as hex) |
| `src/rom_store.c` | A file of the model's ROM size in `~/rpi5emu/roms/` whose checksum names that model (any name) |
| `CMakeLists.txt` | `rpi5emu_embed()` builds in esp32mac's `main/web/macfiles.js` (LGPL) as `macfiles_js` |

Keep `src/main.c` in step with esp32mac's `main/main.c`.

## Build, run, check

```sh
../rpi5emu/tools/pi.sh rpi5mac build
../rpi5emu/tools/pi.sh rpi5mac headless     # or run: the window here
../rpi5emu/tools/console.sh i c
../rpi5emu/tools/pi.sh rpi5mac shot out.png
```

Console: the runtime's (`h`) and esp32mac's: `k <hex>` tap a Mac key code
(24 Return), `K <hex> 0|1`, `m <dx> <dy> <btn>`, `p <x> <y> <btn>` the
pointer at a screen position, `R <model> <ram KB>`, `n` the programmer's
interrupt switch.

Checked on the Pi (2026-11-15, headless): the Plus (4 MB) at 100%, 60
frames a second, to the disk icon asking for a system disk; the SE and the
Classic start. Not yet: a system from a disk (none here), the window by
hand, sound heard, the mouse.
