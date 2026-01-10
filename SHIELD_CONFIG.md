# Skeletyl Shield Configuration Summary

## Quick Reference

### Shield Location
```
boards/shields/skeletyl/
```

### Key Files
- `skeletyl.dtsi` - Base matrix and transform
- `skeletyl_left.overlay` - Left half pin config
- `skeletyl_right.overlay` - Right half pin config
- `skeletyl.zmk.yml` - Shield manifest
- `Kconfig.defconfig` - Split configuration
- `skeletyl.keymap` - Miryoku layout

### Build Commands
```bash
# Individual builds
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_left
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_right

# Both halves
west build -p --board-file build.yaml
```

## Pin Mapping (Pro Micro → nice!nano v2)

### Matrix Pins

| Function | Pro Micro | nice!nano v2 | Zephyr GPIO | Direction |
|----------|-----------|--------------|-------------|-----------|
| R1       | 18        | 18           | P0_03       | Input (Pull Down) |
| R2       | 4         | 7            | P0_02       | Input (Pull Down) |
| R3       | 5         | 8            | P1_15       | Input (Pull Down) |
| R4       | 9         | 12           | P0_09       | Input (Pull Down) |
| C1       | 20        | 19           | P0_28       | Output (High) |
| C2       | 10        | 13           | P1_04       | Output (High) |
| C3       | 6         | 9            | P1_13       | Output (High) |
| C4       | 7         | 10           | P1_11       | Output (High) |
| C5       | 8         | 11           | P0_10       | Output (High) |

### UART (Split Communication)

| Signal | Pro Micro | nice!nano v2 | Zephyr GPIO |
|--------|-----------|--------------|-------------|
| TX     | 1         | 1            | P0_06       |
| RX     | 0         | 0            | P1_08       |

## Matrix Layout

### Left Half (Central)
```
C1 C2 C3 C4 C5
R1 Q  W  E  R  T
R2 A  S  D  F  G
R3 Z  X  C  V  B
R4 ESC TAB SPACE RET BSPC
```

### Right Half (Peripheral)
```
C1 C2 C3 C4 C5
R1 Y  U  I  O  P
R2 H  J  K  L  ;
R3 N  M  ,  .  /
R4 DEL NAV SYM NUM FUN
```

## Configuration Details

### Diode Direction
- **col2row** - Standard PCB
- **row2col** - Amoeba RGB PCB

### GPIO Configuration
- **Rows**: `GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN`
- **Columns**: `GPIO_ACTIVE_HIGH`

### Matrix Transform
- **Left**: No offset (columns 0-4)
- **Right**: Offset of 5 (columns 5-9)

### Split Role
- **Left**: Central (handles USB/BLE)
- **Right**: Peripheral

## Layer Structure (Default Keymap)

1. **BASE** - Main typing layer (QWERTY)
2. **NAV** - Navigation, mouse, arrows
3. **SYM** - Symbols, brackets, operators
4. **NUM** - Numbers, function keys F1-F10
5. **FUN** - Extended function keys F11-F24
6. **MED** - Media controls, power

## ZMK Features Enabled

- **Split Keyboard** - Left/right halves
- **Bluetooth** - nice_nano_v2 wireless
- **USB** - Wired connection support
- **Sleep** - Configurable in `.conf` files
- **OLED** - Supported (optional)

## Keymap Behaviors

### Common Modifiers
- **Hold-Tap (HM)** - Homerow mods (Ctrl, Alt, Gui, Shift)
- **Layer-Tap (LT)** - Access layers with key hold
- **Transparent (trans)** - Fall through to lower layer

### Layer Access
- Raise (RAISE) - Access SYM and NUM layers
- Lower (LOWER) - Access NAV layer
- Adjust (ADJUST) - Access MED layer

## Power Management

### Battery Operation
- **Left**: USB-C or LiPo battery
- **Right**: LiPo battery only (receives power via split connection)

### Sleep Configuration
Enable in `skeletyl.conf`:
```c
CONFIG_ZMK_SLEEP=y
```

## Troubleshooting

### Build Errors

**"shield skeletyl_left not found":**
- Verify `boards/shields/skeletyl/` exists
- Run `west update`

**Pin configuration errors:**
- Check `skeletyl_left.overlay` and `skeletyl_right.overlay`
- Verify GPIO numbers match `docs/PINOUT.md`

**Matrix not scanning:**
- Check diode direction in `skeletyl.dtsi`
- Verify GPIO pull configuration

### Runtime Issues

**Keys not responding:**
- Verify hardware connections
- Check matrix with `zmkfirmware/zmk/tools/zmk-gui-mapper`

**Split not connecting:**
- Check UART wiring (TX/RX/GND)
- Verify left half is central role

**Battery drain:**
- Enable sleep: `CONFIG_ZMK_SLEEP=y`
- Check for stuck keys

## Documentation

- **PINOUT.md** - Detailed pin diagrams and mapping
- **MIGRATION.md** - Migration from external module
- **boards/shields/skeletyl/README.md** - Shield documentation
- **CLAUDE.md** - Project overview and structure

## References

- [ZMK Firmware](https://zmk.dev/)
- [ZMK Shields](https://zmk.dev/docs/development/shields/)
- [Skeletyl Hardware](https://github.com/Bastardkb/Skeletyl)
- [nice!nano v2](https://nicekeyboards.com/nice-nano-v2/)
