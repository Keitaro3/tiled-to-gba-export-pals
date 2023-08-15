# Tiled to GBA export w/ Palettes
This is a modified version of the original tiled-to-gba-export source. It works exactly the same as the original, except this version assumes each tile has been assigned a custom property named "Palette", corresponding to an integer between 0 and 15. This will assign the appropriate BG palette slot to a given tile, allowing for more expansive 4bpp tilesets to be used. The reason for this is that the original exporter simply assigned all tiles to use Palette 0, which is fine if your tileset only uses 16 colors. This will simply allow you the opportunity to have more control over your output, which is something I required for my own project.

For this to work, you MUST assign all tiles a custom property called "Palette". This property must be an integer, and must be between 0 and 15 (going outside this parameter will default to 0 in the output). The number you choose should correspond to a given palette slot entry in your game. I searched for a long time and couldn't find something that did this functionality, so hopefully someone else will find this useful as well!

PLEASE ALSO NOTE: I've currently only updated the exporting of "regular" backgrounds. The affine backgrounds file will be updated soon but otherwise does not currently support palette assignment!

# Orignal README.md
These are a set of extensions/export plugins for the [Tiled map editor](https://www.mapeditor.org/) that add the following types to the "Export As" menu:

* GBA source files - affine (*.c, *.h)
* GBA source files - regular (*.c, *.h)

These options generate C arrays of hexadecimal tile IDs*, that can be loaded directly into GBA VRAM for use as affine or regular tiled backgrounds. This is a simple and easy to use format for Game Boy Advance development.

<sub>* Blank tiles are defaulted to 0x0000</sub>

### Regular backgrounds
Strictly speaking, valid map sizes for regular backgrounds are 32x32, 64x32, 32x64 and 64x64. However, this extension allows you to export maps of any size as long as the width and height are a multiple of 32.

Each tile layer is parsed in 32x32 chunks - a screenblock on GBA.

### Affine backgrounds

Valid map sizes for affine backgrounds are 16x16, 32x32, 64x64 and 128x128. Each tile layer is parsed linearly, unlike regular backgrounds.

## Installation
This extension requires Tiled 1.4 or newer. Get the latest version [here](https://www.mapeditor.org/).

To add this extension to your Tiled installation:
* Open Tiled and go to Edit > Preferences > Plugins and click the "Open" button to open the extensions directory.
* Download [tiled-to-gba-export.js](https://raw.githubusercontent.com/djedditt/tiled-to-gba-export/master/tiled-to-gba-export.js) and [tiled-to-gba-export-affine.js](https://raw.githubusercontent.com/djedditt/tiled-to-gba-export/master/tiled-to-gba-export-affine.js) in this repository and copy them to that location. The scripts can be placed either directly in the extensions directory or in a subdirectory.

## Output
The export plugins generate a .c source file and associated .h header file.

Below you'll find example output for a 64x32 *regular* map with two tile layers. Do note that the array entries here are shortened with "..." and "etc." for demonstration purposes only. The real output would contain 1024 entries per *screenblock*.

**example.h**

```C
#ifndef EXAMPLE_H
#define EXAMPLE_H

#define EXAMPLE_WIDTH  (64)
#define EXAMPLE_HEIGHT (32)
#define EXAMPLE_LENGTH (2048)

extern const unsigned short Tile_Layer_1[2048];
extern const unsigned short Tile_Layer_2[2048];

#endif

```

**example.c**

```C
#include "example.h"

const unsigned short Tile_Layer_1[2048] __attribute__((aligned(4))) =
{
    // Screen block 0
    0x0000, 0x0001, 0x0002, 0x0003, ...
    0x001E, 0x001F, 0x0020, 0x0021, ...
    0x003C, 0x003D, 0x003E, 0x003F, ...
    0x005A, 0x005B, 0x005C, 0x005D, ...
    etc.

    // Screen block 1
    0x0000, 0x0001, 0x0002, 0x0003, ...
    0x001E, 0x001F, 0x0020, 0x0021, ...
    0x003C, 0x003D, 0x003E, 0x003F, ...
    0x005A, 0x005B, 0x005C, 0x005D, ...
    etc.
};

const unsigned short Tile_Layer_2[2048] __attribute__((aligned(4))) =
{
    // Screen block 0
    0x00F8, 0x00F9, 0x00FA, 0x00FB, ...
    0x0116, 0x0117, 0x0118, 0x0119, ...
    0x0134, 0x0135, 0x0136, 0x0137, ...
    0x0152, 0x0153, 0x0154, 0x0155, ...
    etc.

    // Screen block 1
    0x00F8, 0x00F9, 0x00FA, 0x00FB, ...
    0x0116, 0x0117, 0x0118, 0x0119, ...
    0x0134, 0x0135, 0x0136, 0x0137, ...
    0x0152, 0x0153, 0x0154, 0x0155, ...
    etc.
};

```

## License
This work is licensed under the MIT License. See [LICENSE](https://raw.githubusercontent.com/djedditt/tiled-to-gba-export/master/LICENSE) for details.
