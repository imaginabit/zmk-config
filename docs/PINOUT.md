# Skeletyl Keyboard - Pinout Guide

This document provides the pin connections for building the Skeletyl keyboard using Nice Nano V2 (Pro Micro compatible).

## Pro Micro vs nice!nano v2 Reference

### Pinout Diagrams

#### Pro Micro
```
                    +-------+
          (TX) TXO -| 1   24 |- RAW
          (RX) RXI -| 2   23 |- GND
               GND -| 3   22 |- RST
               GND -| 4   21 |- VCC
                2  -| 5   20 |- A3         
                3  -| 6   19 |- A2    C1
          R2    4  -| 7   18 |- A1
          R3    5  -| 8   17 |- A0    R1
          C3    6  -| 9   16 |- 15
          C4    7  -| 10  15 |- 14
          C5    8  -| 11  14 |- 16
          R4    9  -| 12  13 |- 10    C2
                    +--------+
```

#### nice!nano v2
```
                   +--------+
        (B+)  B+  -| 1   24 |- RAW (VCC)
              GND -| 2   23 |- GND
              RST -| 3   22 |- RST
        (VCC) VCC -| 4   21 |- VCC (3.3V)
             P0.31-| 5   20 |- P0.02 (A3) 
             P0.29-| 6   19 |- P0.28 (A2)   COL1   
     ROW2    P0.02-| 7   18 |- P0.03 (A1)
     ROW3    P1.15-| 8   17 |- P0.26 (A0)   ROW1 
     COL3    P1.13-| 9   16 |- P0.10 (15) 
     COL4    P1.11-| 10  15 |- P0.09 (14)
     COL5    P0.10-| 11  14 |- P1.06 (16)
     ROW4    P0.09-| 12  13 |- P1.04 (10)   COL2 
                +--------+
```

### Pin Mapping Reference

| Pro Micro | nice!nano v2 | Zephyr Pin Name | Analog |
|-----------|--------------|-----------------|---------|
| 2         | P0.31        | P0_31           | -       |
| 3         | P0.29        | P0_29           | -       |
| 4         | P0.02        | P0_02           | -       |
| 5         | P1.15        | P1_15           | -       |
| 6         | P1.13        | P1_13           | -       |
| 7         | P1.11        | P1_11           | -       |
| 8         | P0.10        | P0_10           | -       |
| 9         | P0.09        | P0_09           | -       |
| 10        | P1.04        | P1_04           | -       |
| 14        | P0.09        | P0_09           | -       |
| 15        | P0.10        | P0_10           | -       |
| 16        | P1.06        | P1_06           | -       |
| A0        | P0.26        | P0_26           | A0      |
| A1        | P0.03        | P0_03           | A1      |
| A2        | P0.28        | P0_28           | A2      |
| A3        | P0.02        | P0_02           | A3      |
| RAW/VCC   | RAW/VCC      | VCC             | -       |
| GND       | GND          | GND             | -       |
| RST       | RST          | RST             | -       |
| TXO       | -            | -               | -       |
| RXI       | -            | -               | -       |

**Important Notes:**
- The nice!nano v2 uses Nordic Semiconductor's GPIO naming convention (P0_XX, P1_XX)
- Pro Micro uses simpler sequential numbering (0-23)
- When configuring ZMK firmware for nice!nano v2, use the Zephyr pin names (P0_31, P0_29, etc.)
- VCC pin provides 3.3V on nice!nano v2
- Both boards support the same basic functions but with different pin identifiers

## Pin Configuration

### Left Side (Central) y Right Side (Peripheral)

| Function    | Pro Micro Pin | nice!nano v2 Pin | GPIO (Zephyr) |
|-------------|---------------|------------------|---------------|
| R1 (ROW0)  | 18            | 18               | P0_03         |
| R2 (ROW1)  | 4             | 7                | P0_02         |
| R3 (ROW2)  | 5             | 8                | P1_15         |
| R4 (ROW3)  | 9             | 12               | P0_09         |
| C1 (COL0)  | 20            | 19               | P0_28         |
| C2 (COL1)  | 10            | 13               | P1_04         |
| C3 (COL2)  | 6             | 9                | P1_13         |
| C4 (COL3)  | 7             | 10               | P1_11         |
| C5 (COL4)  | 8             | 11               | P0_10         |

## Matrix Layout (4x5 per side)

### Left Half (20 keys)
```
     C1    C2    C3    C4    C5
     ────  ────  ────  ────  ────
R1   Q     W     E     R     T
R2   A     S     D     F     G
R3   Z     X     C     V     B
R4   ESC   -     RET   BSPC      -
```

### Right Half (20 keys)
```
     C1    C2    C3    C4    C5
     ────  ────  ────  ────  ────
R1   Y     U     I     O     P
R2   H     J     K     L     ;
R3   N     M     ,     .     /
R4   DEL   -     NAV   SYM   -
```
## Skeletyl Keyboard Connections

### Pin Mapping for Skeletyl

| Pro Micro Pin | nice!nano v2 Pin | GPIO (Zephyr) | Function | Usage |
|---------------|------------------|---------------|----------|-------|
| 18            | 18               | P0_03         | R1       | Row 1 (Top row) |
| 4             | 7                | P0_02         | R2       | Row 2 |
| 5             | 8                | P1_15         | R3       | Row 3 |
| 9             | 12               | P0_09         | R4       | Row 4 (Bottom row) |
| 20            | 19               | P0_28         | C1       | Column 1 (Leftmost) |
| 10            | 13               | P1_04         | C2       | Column 2 |
| 6             | 9                | P1_13         | C3       | Column 3 |
| 7             | 10               | P1_11         | C4       | Column 4 |
| 8             | 11               | P0_10         | C5       | Column 5 (Rightmost) |
| 1             | 1                | P0_06         | UART TX  | Communication with other half |
| 0             | 0                | P1_08         | UART RX  | Communication with other half |

**Note:** When using ZMK firmware, reference pins by their GPIO names (P0_03, P0_02, etc.) rather than their pin numbers (18, 4, etc.).

## Power Distribution

| Source | Left | Right                       |
| ------ | ---- | --------------------------- |
| VCC    | 3.3V | 3.3V (from battery or left) |
| GND    | GND  | GND                         |

For battery powered split keyboards:
- Left side: USB or battery
- Right side: Battery only (receives power via split connection or separate battery)

## Typical Build Options

### Using Amoeba RGB PCB
- Add `GPIO_PULL_DOWN` to columns instead of rows
- Enable RGB underglow

### Using Standard PCB
- Rows with `GPIO_PULL_DOWN`
- Columns as outputs

## Reference

- Original shield: [zmk-config-handwired-skeletyl](https://github.com/cyanindya/zmk-config-handwired-skeletyl)
- Nice Nano V2 documentation
- ZMK Firmware: https://zmk.dev/
