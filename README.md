         _____  __ _  _   _      _ _
        / ____|/ /| || | | |    (_) |
       | |    / /_| || |_| |     _| |__
       | |   | '_ \__   _| |    | | '_ \
       | |___| (_) | | | | |____| | |_) |
        \_____\___/  |_| |______|_|_.__/



#### Introduction

This is a collection of libraries for making C64 development more enjoyable using KickAssembler [http://theweb.dk/KickAssembler/Main.html#frontpage]

It contains pseudocommands, macros, constants and functions.

#### Using C64Lib

To use from your own project, clone this repository and reference the cloned directory using the `-libdir` KickAssembler command line parameter.

```bash
java –jar kickass.jar your_main.asm -libdir ~/Git/c64lib/
```

You can also use my KickAssembler docker container:

```bash
docker run -v ${PWD}:/workspace barrywalker71/kickassembler:latest /workspace/your_main.asm
```

Specify the location of the cloned directory in the `-libdir` parameter. In this example, it's `~/Git/c64lib`, but use whatever location you cloned the directory to.


In your `your_main.asm` file, include either the `include_me_full.asm` or the `include_me_min.asm` files:

```asm
#include "include_me_full.asm"
```

or

```asm
#include "include_me_min.asm"
```

The `include_me_min.asm` file only brings in the pseudocommands, macros and constants. Inclding `include_me_full.asm` will also bring in sprites, timer and memory routines.

You can also include individual features:

- include_me_memory.asm
- include_me_sprites.asm
- include_me_timers.asm

#### What's included?

Bringing in the `include_me_min.asm` file gives you access to:

- All of the Kernal constants. (`kernal` namespace)
- All of the BASIC constants. (`basic` namespace)
- All of the VIC-II constants. (`vic` namespace)
- The constants for the 32 addresses of the twin CIAs (CIA1 and CIA2). (`cia` namespace)
- 29 constants for the SID chip. (`sid` namespace)
- A bunch of pseudocommands to make your code easier to write and more consistent.
- A bunch of macros to further ease development.
- Constants used by the library that can also be used in your own code.
- 8 16-bit zeropage pseudo registers modeled after the way the GEOS guys did it (r0, r0L, r0H, etc). NOTE: If you're using BASIC, these registers will conflict with addresses in use by it.

Using `include_me_full.asm` brings in these additional features:

- Sprite routines
- Memory fill and copy routines
- Timer routines for single-shot and continuous timers

__NOTE__ There is a hardware register version of the sprite routines if you can't spare, or can't use ZP memory. You can include `include_me_sprites_r.asm` to use it. The sprite macros below will automatically adapt and are called the same way as the pseudo register versions.

I will work on making hardware register versions of the other routines as well.

#### What's available for sprites?

Start by including `include_me_sprites.asm` in your project. If you need access to other routines, include `include_me_full.asm`

##### Enabling a sprite

```asm
SpriteEnable(0, TRUE)
```

or

```asm
SpriteEnable(0, FALSE)
```

##### Setting a sprite's priority

```asm
SpritePriority(0, SPR_BG)
```

or

```asm
SpritePriority(0, SPR_FG)
```

##### Setting a sprite's X/Y expand

```asm
SpriteXExpand(0, SPR_NORMAL)
```

or

```asm
SpriteYExpand(0, SPR_EXPAND)
```

##### Setting a sprite's color mode

```asm
SpriteColorMode(0, SPR_HIRES)
```

or

```asm
SpriteColorMode(0, SPR_MULTICOLOR)
```

##### To move a sprite

```asm
PositionSprite(0, spr_x_pos, spr_y_pos)
```

Store the sprite's x position (0-319) in the word at `spr_x_pos` and the sprite's y position (0-255) in the byte at `spr_y_pos`.

This routine automatically handles the sprite's MSB when the x position is > 255
