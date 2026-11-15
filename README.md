# rpi5mac

A Macintosh Plus, SE or Classic for the Raspberry Pi 5, in a window of its
own: [esp32mac](https://github.com/rlucencic/esp32mac)'s Mac (with
esp32emu's 68000, Musashi) on the [rpi5emu](https://github.com/rlucencic/rpi5emu)
runtime.

- The 68000, up to 4 MB, classic Mac OS from disk images of any size (DSK,
  IMG, IMAGE, HFV, HDA, ISO, TOAST) in three drives.
- The setup screen (model, memory, sound, the drives), the web page for
  the disk images (StuffIt, Compact Pro, BinHex and MacBinary files dropped
  on it become HFS disks in the browser), the console, as the rest of the
  family.

## Building

See [rpi5emu](https://github.com/rlucencic/rpi5emu#building): esp32emu,
rpi5emu, esp32mac and rpi5mac cloned side by side, then

```sh
cd rpi5mac && cmake -S . -B build -G Ninja && cmake --build build
```

## ROMs

The Mac Plus's (128 KB), the SE's (256 KB) or the Classic's (512 KB) in the
data folder's `roms/` (`~/rpi5emu/roms`), under any name (esp32mac's are
`plus.rom`, `se.rom`, `classic.rom`): it is found by its checksum. Not part
of this repository.

## Running

```sh
build/rpi5mac                            # the setup screen first
build/rpi5mac -S --choice Model="Mac SE"
build/rpi5mac System6.dsk                # with a disk in drive 1
```

Disk images live in `~/rpi5emu/disks/`. The keyboard by position (US); the
mouse after a click in the window.

## Licence

MIT. The web page's converters (esp32mac's `main/web/macfiles.js`) are
LGPL.
