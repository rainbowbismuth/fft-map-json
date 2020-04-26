# fft-map-json

This repository contains the game play relevant info for each map in FFT. Every map is here except for MAP000 and MAP062
which currently have errors with the code I'm using.

Thanks to the Ganesha FFT Map editor project for the code to parse these files.

## Format

The top level object in each JSON file has the following properties:

- `gns` - The name of the GNS file.
- `lower` - A list of lists containing tile objects. This is the 'lower level' of the map where most of the data is.
- `upper` - Same format and dimensions as `lower`, this contains data for tiles that are above other tiles (like a
bridge)
- `width` - The integer width of the tile data, same as the length of `lower` and `upper`.
- `height` - The integer height of the tile data, same as the length of each list in `lower` and `upper`
- `surface_types` - The unique set of each surface type that appears in the map, for convenience.
- `starting_locations` - The starting locations for each unit on this map.

Each tile object has the following properties:

- `x` & `y` - The coordinates of the tile, as if you were accessing the nested lists like `layer[y][x]`.
- `no_cursor` - `true` if you can't cursor on this tile in the game.
- `no_walk` - `true` if a character can't walk on this tile in the game.
- `depth` - The depth of the water (or lava? Not sure.)
- `height` - The height of the tile.
- `slope_type` - A string corresponding the slope type; Flat, Incline, Convex, Concave plus a cardinal direction. Might
be `null` if the number didn't correspond to something in the list at the end of this README.
- `slope_type_numeric` - The byte for this slope type, never null. Non-zero value might be present
even if `slope_type` is null.
- `surface_type` - A string, like 'Ivy' or 'Road'. Could be null.
- `surface_type_numeric` - The byte for the surface type, similar to `surface_type_numeric`.
- `slope_height` - The height of the slope.

Each starting location object has the following properties:

- `x` & `y` - The coordinates of the tile this unit starts on. (**Currently missing which layer though!**)
- `facing` - The direction the unit starts out facing, either 'North', 'East', 'South' or 'West'.
- `team` - Either 'Player 1' or 'Player 2'
- `unit` - Which unit on the team this starting location is for, starts at 0, goes up to 3 for 4 units total.

## Slope types

```python
SLOPE_TYPES = {
     0x00: 'Flat 0',
     0x85: 'Incline N',
     0x52: 'Incline E',
     0x25: 'Incline S',
     0x58: 'Incline W',
     0x41: 'Convex NE',
     0x11: 'Convex SE',
     0x14: 'Convex SW',
     0x44: 'Convex NW',
     0x96: 'Concave NE',
     0x66: 'Concave SE',
     0x69: 'Concave SW',
     0x99: 'Concave NW',
}
```

## Surface types

```python
SURFACE_TYPES = {
    0x00: "Natural Surface",
    0x01: "Sand area",
    0x02: "Stalactite",
    0x03: "Grassland",
    0x04: "Thicket",
    0x05: "Snow",
    0x06: "Rocky cliff",
    0x07: "Gravel",
    0x08: "Wasteland",
    0x09: "Swamp",
    0x0A: "Marsh",
    0x0B: "Poisoned marsh",
    0x0C: "Lava rocks",
    0x0D: "Ice",
    0x0E: "Waterway",
    0x0F: "River",
    0x10: "Lake",
    0x11: "Sea",
    0x12: "Lava",
    0x13: "Road",
    0x14: "Wooden floor",
    0x15: "Stone floor",
    0x16: "Roof",
    0x17: "Stone wall",
    0x18: "Sky",
    0x19: "Darkness",
    0x1A: "Salt",
    0x1B: "Book",
    0x1C: "Obstacle",
    0x1D: "Rug",
    0x1E: "Tree",
    0x1F: "Box",
    0x20: "Brick",
    0x21: "Chimney",
    0x22: "Mud wall",
    0x23: "Bridge",
    0x24: "Water plant",
    0x25: "Stairs",
    0x26: "Furniture",
    0x27: "Ivy",
    0x28: "Deck",
    0x29: "Machine",
    0x2A: "Iron plate",
    0x2B: "Moss",
    0x2C: "Tombstone",
    0x2D: "Waterfall",
    0x2E: "Coffin",
    0x2F: "(blank)",
    0x30: "(blank)",
    0x3F: "Cross section"
}
```
