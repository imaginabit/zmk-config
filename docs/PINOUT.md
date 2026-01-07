# Skeletyl Keyboard - Pinout Guide

This document provides the pin connections for building the Skeletyl keyboard using Nice Nano V2 (Pro Micro compatible).

## Pin Configuration

### Left Side (Central)

| Function | Nice Nano Pin | GPIO  |
| -------- | ------------- | ----- |
| ROW0     | 19            | P1.11 |
| ROW1     | 18            | P1.10 |
| ROW2     | 15            | P1.08 |
| ROW3     | 14            | P1.07 |
| COL0     | 8             | P0.08 |
| COL1     | 7             | P0.06 |
| COL2     | 6             | P0.07 |
| COL3     | 5             | P0.05 |
| COL4     | 4             | P0.04 |

### Right Side (Peripheral)

| Function | Nice Nano Pin | GPIO  |
| -------- | ------------- | ----- |
| ROW0     | 4             | P0.04 |
| ROW1     | 5             | P0.05 |
| ROW2     | 6             | P0.07 |
| ROW3     | 7             | P0.06 |
| COL5     | 19            | P1.11 |
| COL6     | 18            | P1.10 |
| COL7     | 15            | P1.08 |
| COL8     | 14            | P1.07 |
| COL9     | 16            | P1.06 |

## Matrix Layout (5x4)

### Left Half (10 keys)
```
     COL0  COL1  COL2  COL3  COL4
     ────  ────  ────  ────  ────
ROW0  Q     W     E     R     T
ROW1  A     S     D     F     G
ROW2  Z     X     C     V     B
ROW3  ESC   SPACE TAB   RET   BSPC
```

### Right Half (10 keys)
```
     COL5  COL6  COL7  COL8  COL9
     ────  ────  ────  ────  ────
ROW0  Y     U     I     O     P
ROW1  H     J     K     L     ;
ROW2  N     M     ,     .     /
ROW3  DEL   FUN   NAV   SYM   NUM
```

## Split Connection (UART)

Connect the two halves together for wireless communication:

```
Left (Central)                    Right (Peripheral)
┌────────────────┐               ┌────────────────┐
│                │               │                │
│  TX (Pin 1) ───┼───────────────┼── RX (Pin 0)   │
│                │               │                │
│  RX (Pin 0) ───┼───────────────┼── TX (Pin 1)   │
│                │               │                │
│  GND ──────────┼───────────────┼── GND          │
│                │               │                │
└────────────────┘               └────────────────┘
```

| Signal | Left Pin | Right Pin |
| ------ | -------- | --------- |
| TX     | 1        | 0         |
| RX     | 0        | 1         |
| GND    | GND      | GND       |

## Nice Nano V2 Pinout Diagram

```
          ┌──────────-───────────────┐
       GND│ ○                       ○│ VCC (+3.3V)
        21│ ○                       ○│ 19  ← ROW0 (L) / COL5 (R)
        20│ ○                       ○│ 18  ← ROW1 (L) / COL6 (R)
  ROW2  15│ ○                       ○│ 14  ← ROW3 (L) / COL8 (R)
  ROW3  14│ ○                       ○│ 16  ← COL9 (R)
         9│ ○        ○   ○          ○│ 10
  COL0   8│ ○                       ○│ TX  ← RX (R)
  COL1   7│ ○                       ○│ RX  ← TX (R)
  COL2   6│ ○                       ○│ GND ← GND
  COL3   5│ ○                       ○│
  COL4   4│ ○                       ○│
          └──────────────────-───────┘
             (Top View)
```

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
