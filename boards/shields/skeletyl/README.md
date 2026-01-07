# Skeletyl Keyboard Shield

Local shield definition for the Skeletyl keyboard, extracted from the external module `zmk-config-handwired-skeletyl`.

## Overview

The Skeletyl is a split ergonomic keyboard with 40% layout (4 rows × 5 columns per side).

## Shield Structure

This local shield includes the following files:

- **`skeletyl.dtsi`** - Base device tree include with matrix transform and kscan definition
- **`skeletyl_left.overlay`** - Pin configuration for the left half (central)
- **`skeletyl_right.overlay`** - Pin configuration for the right half (peripheral)
- **`skeletyl.zmk.yml`** - Shield manifest with metadata
- **`Kconfig.defconfig`** - Kconfig definitions for split configuration
- **`Kconfig.shield`** - Shield registration
- **`skeletyl.keymap`** - Default keymap (Miryoku-style layout)
- **`*.conf`** - Configuration overlays

## Pin Configuration

### Rows (GPIO_PULL_DOWN)
| Function | Pro Micro | nice!nano v2 | Zephyr GPIO |
|---------|-----------|--------------|-------------|
| R1      | 18        | 18           | P0_03       |
| R2      | 4         | 7            | P0_02       |
| R3      | 5         | 8            | P1_15       |
| R4      | 9         | 12           | P0_09       |

### Columns (GPIO_ACTIVE_HIGH)
| Function | Pro Micro | nice!nano v2 | Zephyr GPIO |
|---------|-----------|--------------|-------------|
| C1      | 20        | 19           | P0_28       |
| C2      | 10        | 13           | P1_04       |
| C3      | 6         | 9            | P1_13       |
| C4      | 7         | 10           | P1_11       |
| C5      | 8         | 11           | P0_10       |

### UART Communication (Split)
| Signal | Pro Micro | nice!nano v2 | Zephyr GPIO |
|--------|-----------|--------------|-------------|
| TX     | 1         | 1            | P0_06       |
| RX     | 0         | 0            | P1_08       |

## Matrix Layout

### Left Half (20 keys)
```
     C1    C2    C3    C4    C5
     ────  ────  ────  ────  ────
R1   Q     W     E     R     T
R2   A     S     D     F     G
R3   Z     X     C     V     B
R4   ESC   TAB   SPACE RET   BSPC
```

### Right Half (20 keys)
```
     C1    C2    C3    C4    C5
     ────  ────  ────  ────  ────
R1   Y     U     I     O     P
R2   H     J     K     L     ;
R3   N     M     ,     .     /
R4   DEL   NAV   SYM   NUM   FUN
```

## Building

To build firmware for this shield:

```bash
# Build left half
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_left

# Build right half
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_right
```

Or using the build matrix:
```bash
west build -p --board-file build.yaml
```

## Keymap Structure

The default keymap follows the Miryoku layout with these layers:

- **BASE** - Main typing layer (Colemak or QWERTY)
- **NAV** - Navigation and mouse keys
- **SYM** - Symbols and special characters
- **NUM** - Number row and function keys
- **FUN** - Function keys and media controls
- **MED** - Media and power controls

## Configuration Options

Available Kconfig settings in `.conf` files:

- `CONFIG_ZMK_SLEEP=y` - Enable sleep functionality
- Additional options available in `skeletyl.conf`

## Differences from External Module

This local shield replaces the need for the external `zmk-config-handwired-skeletyl` module. All necessary files have been copied and configured locally in `boards/shields/skeletyl/`.

The pin configuration has been verified against the official documentation and uses the correct GPIO mappings for nice!nano v2.

## References

- [ZMK Documentation](https://zmk.dev/docs/)
- [ZMK Firmware GitHub](https://github.com/zmkfirmware/zmk)
- [Original Skeletyl Shield](https://github.com/Bastardkb/Skeletyl)
