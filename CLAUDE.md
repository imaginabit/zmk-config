# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a ZMK (Zephyr™ Mechanical Keyboard) firmware configuration repository for building custom keyboard firmware. ZMK is an open-source keyboard firmware built on the Zephyr RTOS, focused on wireless functionality and modern features.

The repository builds firmware for two keyboard configurations:
- **a_dux**: A split keyboard (uses built-in shield in ZMK)
- **skeletyl**: A compact keyboard layout (uses external module: `zmk-config-handwired-skeletyl`)

## Repository Structure

```
zmk-config/
├── build.yaml              # GitHub Actions build matrix configuration
├── west.yml                # West manifest for Zephyr modules
├── .zmk/
│   └── zmk/               # ZMK core framework (submodule)
│   ├── app/               # ZMK application code
│   └── modules/           # External ZMK modules
│       └── zmk-config-handwired-skeletyl/  # Skeletyl keyboard module
├── config/
│   ├── a_dux.keymap       # a_dux keymap configuration
│   ├── a_dux.conf         # a_dux Kconfig settings
│   ├── skeletyl.keymap    # Skeletyl keymap configuration
│   └── skeletyl.conf      # Skeletyl Kconfig settings
└── boards/
    └── shields/           # Custom shield definitions (if any)
```

## Architecture

### Key Components

1. **ZMK Core Framework** (`.zmk/zmk/`): The main ZMK firmware codebase as a git submodule
2. **External Module** (`zmk-config-handwired-skeletyl`): Custom keyboard definition imported via west.yml
3. **Board/Shield Model**: ZMK uses a board (MCU) + shield (keyboard) architecture
   - Boards: `nice_nano`, `nice_nano_v2` (wireless microcontrollers)
   - Shields: `a_dux`, `skeletyl` (keyboard layouts)

### Configuration Files

- **build.yaml**: Defines which board+shield combinations to build in CI
- **west.yml**: West manifest specifying ZMK version and external modules
- **\*.keymap**: Keymap definitions using ZMK's devicetree syntax
- **\*.conf**: Kconfig configuration overlays (e.g., sleep settings, features)

## Building Firmware

### Prerequisites

The repository uses Zephyr's build system (west/cmake/ninja):

1. Install Zephyr development environment (see [ZMK Setup](https://zmk.dev/docs/development/setup/))
2. Install west: `pip3 install west`
3. Initialize workspace: `west init -l config`
4. Update modules: `west update`
5. Build: `west build -p -b <board> -- -DSHIELD=<shield>`

### Build Commands

```bash
# Build a_dux firmware
west build -p -b nice_nano -- -DSHIELD=a_dux_left
west build -p -b nice_nano -- -DSHIELD=a_dux_right

# Build skeletyl firmware
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_left
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_right

# Build with automatic configuration
west build -p --board-file build.yaml
```

### CI/CD

GitHub Actions (`.github/workflows/build.yml`) automatically builds all board+shield combinations defined in `build.yaml` on every push and pull request.

## Customization

### Modifying Keymaps

Keymaps are defined in `config/*.keymap` using ZMK's devicetree syntax:

- **`skeletyl.keymap`**: Miryoku-style layout with layers: BASE, NAV, SYM, NUM, FUN, MED
- **`a_dux.keymap`**: Split keyboard layout

Keymap layers reference behaviors defined in the devicetree. Common patterns:
- `&kp <key>`: Key press behavior
- `&hm <mod> <key>`: Hold-tap for homerow mods
- `&lt <layer> <key>`: Layer-tap behavior
- `&trans`: Transparent (fall through to layer below)

### Configuration Options

Kconfig settings in `*.conf` files:
- `CONFIG_ZMK_SLEEP=y`: Enable sleep functionality
- Additional options: See [ZMK Config](https://zmk.dev/docs/config/)

### Adding New Keyboards

1. Add shield definition to ZMK or external module
2. Update `build.yaml` with new board+shield combination
3. Create keymap and conf files in `config/`

## Code Quality

The repository uses pre-commit hooks (`.zmk/zmk/.pre-commit-config.yaml`):

```bash
# Install pre-commit hooks
pip3 install pre-commit
pre-commit install

# Run manually
pre-commit run --all-files
```

Checks include:
- **clang-format**: C/C++ code formatting
- **prettier**: YAML/Markdown formatting
- **gitlint**: Git commit message style
- **check-yaml**: YAML syntax validation
- **trailing-whitespace**: Remove trailing spaces

## External Dependencies

### West Manifest

`config/west.yml` specifies:
- ZMK version: `v0.3`
- External modules: `zmk-config-handwired-skeletyl`
- Zephyr base: https://github.com/zmkfirmware/zmk

### Module Updates

To update external modules:
```bash
west update
```

## Important Files to Know

- `build.yaml:1-30`: Build matrix configuration for CI
- `config/west.yml:1-19`: Module manifest (ZMK version, external modules)
- `config/skeletyl.keymap:1-85`: Skeletyl keymap (Miryoku layout)
- `config/skeletyl.conf:1`: Configuration overlay
- `.github/workflows/build.yml:1-6`: CI build pipeline
- `token:1-3`: Contains GitHub tokens (DO NOT COMMIT SENSITIVE DATA)

## Common Issues & Solutions

1. **Build failures**: Run `west update` to sync submodules
2. **Keymap syntax errors**:
   - Missing closing parentheses for macros (e.g., `&kp LC(Y)` not `&kp LC(Y`)
   - Unclosed devicetree lists (ensure `<` has matching `>`)
   - Validate devicetree syntax, check ZMK docs
3. **Missing shield**: Ensure external module is properly referenced in west.yml
4. **CI build fails**: Check build.yaml syntax, ensure all referenced boards/shields exist
5. **"unterminated argument list" errors**: Check for missing closing brackets `)` in keymap macros
6. **External module not found**: Ensure the module repository is public and accessible

## Resources

- [ZMK Documentation](https://zmk.dev/docs/)
- [ZMK Firmware GitHub](https://github.com/zmkfirmware/zmk)
- [Zephyr Documentation](https://docs.zephyrproject.org/)
- [ZMK Discord Community](https://zmk.dev/community/discord/invite)
