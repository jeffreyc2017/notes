# bochs

<https://github.com/bochs-emu/Bochs>
<https://github.com/lubomyr/bochs>
<https://wiki.osdev.org/Bochs>

## Installation

### Ubuntu

```sh
sudo apt install bochs bochsbios vgabios bochs-x bochs-sdl bochs-term bochs-doc
```

Docs after installing `bochs-doc`: `/usr/share/doc/bochs`

version:

```sh
========================================================================
                        Bochs x86 Emulator 2.7
              Built from SVN snapshot on August  1, 2021
                Timestamp: Sun Aug  1 10:07:00 CEST 2021
========================================================================
```

Issues:

- [Black screen issue on Ubuntu 22.04](https://stackoverflow.com/questions/73067357/bochs-can-not-load-bootloader-using-a-floppy-image/73084308#73084308)

> On Ubuntu 22.04 I also get bx_dbg_read_linear: physical memory read error (phy=0x0000322f3130, lin=0x00000000322f3130). This is due to a bad BIOS-bochs-latest ROM image. Replacing this with either BIOS-bochs-legacy from the Ubuntu package, or BIOS-bochs-latest from a more recent
