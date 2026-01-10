# Migration: External Module to Local Shield

This document describes the migration from external module `zmk-config-handwired-skeletyl` to local shield definition.

## Changes Made

### 1. File Structure

**Before (External Module):**
```
config/
├── west.yml                  # Referenced external module
└── modules/
    └── zmk-config-handwired-skeletyl/
        └── boards/shields/skeletyl/
            ├── skeletyl.dtsi
            ├── skeletyl_left.overlay
            ├── skeletyl_right.overlay
            ├── skeletyl.zmk.yml
            ├── Kconfig.defconfig
            ├── Kconfig.shield
            ├── skeletyl.keymap
            └── *.conf
```

**After (Local Shield):**
```
config/
├── west.yml                  # External module commented out
├── build.yaml                 # Build matrix (unchanged)
└── boards/shields/skeletyl/   # Local shield definition
    ├── skeletyl.dtsi
    ├── skeletyl_left.overlay
    ├── skeletyl_right.overlay
    ├── skeletyl.zmk.yml
    ├── Kconfig.defconfig
    ├── Kconfig.shield
    ├── skeletyl.keymap
    ├── *.conf
    └── README.md              # New documentation
```

### 2. west.yml Changes

**Removed:**
```yaml
- name: zmk-config-handwired-skeletyl
  url: https://github.com/cyanindya/zmk-config-handwired-skeletyl
  revision: master
  path: modules/zmk-config-handwired-skeletyl
```

**Replaced with:**
```yaml
# Local shield definition - moved to boards/shields/skeletyl/
#  - name: zmk-config-handwired-skeletyl
#    url: https://github.com/cyanindya/zmk-config-handwired-skeletyl
#    revision: master
#    path: modules/zmk-config-handwired-skeletyl
```

### 3. Pin Configuration Updates

All pin configurations were updated to match the official Skeletyl documentation:

**Rows (Pro Micro → nice!nano v2):**
- R1: 18 → P0_03
- R2: 4 → P0_02
- R3: 5 → P1_15
- R4: 9 → P0_09

**Columns (Pro Micro → nice!nano v2):**
- C1: 20 → P0_28
- C2: 10 → P1_04
- C3: 6 → P1_13
- C4: 7 → P1_11
- C5: 8 → P0_10

### 4. Documentation Updates

**Updated files:**
- `docs/PINOUT.md` - Corrected pin mappings and GPIO assignments
- `docs/PINOUT.md` - Unified pin configuration tables
- `boards/shields/skeletyl/README.md` - New comprehensive shield documentation

## Benefits of Local Shield

1. **No External Dependencies** - Self-contained configuration
2. **Faster Builds** - No need to fetch external module
3. **Customizable** - Easy to modify without forking external repo
4. **Version Control** - All changes tracked in main repository
5. **Documentation** - All documentation centralized

## Verification

To verify the migration was successful:

1. **Check file structure:**
   ```bash
   ls -la boards/shields/skeletyl/
   ```

2. **Verify west.yml:**
   ```bash
   grep -A 4 "zmk-config-handwired-skeletyl" config/west.yml
   ```

3. **Test build:**
   ```bash
   west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_left
   ```

## Rollback

To rollback to external module:

1. Uncomment the external module section in `config/west.yml`
2. Run `west update`
3. Remove local shield directory: `rm -rf boards/shields/skeletyl/`

## Build Commands

**Left half:**
```bash
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_left
```

**Right half:**
```bash
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_right
```

**Both halves (using build matrix):**
```bash
west build -p --board-file build.yaml
```

## Troubleshooting

### Build Fails: "shield not found"

Ensure the shield directory exists:
```bash
test -d boards/shields/skeletyl && echo "Shield found" || echo "Shield missing"
```

### Module Still Referenced

Clear west build directory:
```bash
rm -rf build/ && west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_left
```

### Pin Configuration Issues

Verify pin assignments match `docs/PINOUT.md`:
```bash
grep -A 10 "Pin Configuration" docs/PINOUT.md
```

## References

- [ZMK Shield Documentation](https://zmk.dev/docs/development/shields/)
- [Zephyr Devicetree](https://docs.zephyrproject.org/latest/guides/dts/index.html)
- [Original Skeletyl Module](https://github.com/cyanindya/zmk-config-handwired-skeletyl)
