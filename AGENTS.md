# AGENTS.md

This file provides guidance for agentic coding assistants working in this ZMK keyboard firmware configuration repository.

## Build Commands

### Build Firmware
```bash
# Build all configurations from build.yaml
west build -p --board-file build.yaml

# Build specific keyboard (skeletyl)
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_left
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_right

# Build a_dux split keyboard
west build -p -b nice_nano -- -DSHIELD=a_dux_left
west build -p -b nice_nano -- -DSHIELD=a_dux_right

# Clean build
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_left -DCLEAN_BUILD
```

### Update Dependencies
```bash
west update
```

### Run Pre-commit Linting
```bash
pip3 install pre-commit
pre-commit install
pre-commit run --all-files
pre-commit run --all-files --show-diff-on-failure
```

## Code Style Guidelines

### Keymap Files (*.keymap)

Keymaps use DeviceTree syntax (C-like). Follow these conventions:

**Includes Order:**
```c
#include <behaviors.dtsi>
#include <dt-bindings/zmk/keys.h>
#include <dt-bindings/zmk/modifiers.h>
```

**Layer Definitions:**
- Use `#define` for layer numbers at the top
- Layer 0 = BASE (default), 1 = NAV, 2 = SYM, 3 = NUM, 4 = FUN, 5 = MED
- Name layers descriptively (nav_layer, sym_layer, etc.)

**Bindings Format:**
- Use tabs for indentation
- Keep keymaps readable with proper spacing
- Use `&kp` for key press, `&hm` for homerow mods, `&lt` for layer-tap
- Macros: `&kp LC(Y)` for Left Ctrl+Y (always close parentheses)

**Homerow Mods:**
```c
&hm LGUI A  &hm LALT S  &hm LCTRL D  &hm LSHFT F
```

**Transparent Keys:**
- Use `&trans` to pass keypress to underlying layer
- Use `&none` for disabled keys

### Configuration Files (*.conf)

Kconfig overlay files:
- Use `CONFIG_NAME=y` for enabled options
- One setting per line
- Comment only when necessary for non-obvious settings

### YAML Files (build.yaml, west.yml)

- 2-space indentation
- No trailing commas
- Use quotes for strings with special characters
- Keep lists in flow style when short

## Import Guidelines

**Required Headers for Keymaps:**
```c
#include <behaviors.dtsi>
#include <dt-bindings/zmk/keys.h>
#include <dt-bindings/zmk/modifiers.h>
```

**External Module References:**
- Add to `config/west.yml` under `projects:`
- Include `url` and `revision` fields
- Specify `path` to avoid conflicts

## Naming Conventions

- **Keymap files**: `{keyboard}.keymap` (lowercase, underscores)
- **Config files**: `{keyboard}.conf` (lowercase, underscores)
- **Layer names**: snake_case (nav_layer, sym_layer)
- **Behavior labels**: UPPER_SNAKE_CASE with descriptive names
- **Shield names**: kebab-case (skeletyl_left, skeletyl_right)

## Error Handling

**Common Build Errors:**
- `"unterminated argument list"`: Missing `)` in macros like `&kp LC(Y)`
- `"missing or invalid devicetree binding"`: Check include paths
- `"shield not found"`: Verify west.yml module reference and run `west update`

**Keymap Validation:**
- Ensure `<` has matching `>` in bindings list
- Each layer must have `compatible = "zmk,keymap"`
- Behavior nodes need unique `label` properties

## Critical Files

- `config/skeletyl.keymap:1-86` - Miryoku layout with 6 layers
- `config/west.yml:1-19` - ZMK version (v0.3) and external modules
- `build.yaml:21-26` - Build matrix for CI
- `.zmk/zmk/.pre-commit-config.yaml` - Linting configuration

## CI/CD

GitHub Actions builds from `build.yaml` on every push/PR. No manual test commands - CI validates builds automatically.

## Resources

- [ZMK Documentation](https://zmk.dev/docs/)
- [ZMK Keymap Example](https://zmk.dev/docs/keymap-example/)
- [ZMK Behaviors](https://zmk.dev/docs/keymaps/behaviors/)
- [DeviceTree Syntax](https://docs.zephyrproject.org/latest/build/dts/intro.html)
