# Testing Teclado - Verificación de Mapeo de Teclas

## Objetivo

Verificar si la configuración actual de pines coincide con el hardware real del Skeletyl.

## Métodos de Testing

### Método 1: ZMK Studio (Recomendado)

**Ventajas:**
- Interfaz gráfica fácil
- Detección automática
- Feedback inmediato

**Pasos:**
```bash
# 1. Build con ZMK Studio
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_left

# 2. Flash
west flash

# 3. Abrir ZMK Studio
# - Descargar: https://zmk.dev/studio
# - Conectar teclado via USB
# - Presionar cada tecla
# - Anotar qué tecla se registra vs esperada
```

**Qué buscar:**
- ✅ Si presionas "Q" y aparece "Q" → Pin correcto
- ❌ Si presionas "Q" y aparece "W" → Pin incorrecto
- ❌ Si presionas "Q" y no pasa nada → Pin mal configurado

### Método 2: Test Keymap

**Archivo creado:** `boards/shields/skeletyl/test_keymap.keymap`

**Uso:**
```bash
# Backup
cp config/skeletyl.keymap config/skeletyl.keymap.backup

# Usar test keymap
cp boards/shields/skeletyl/test_keymap.keymap config/skeletyl.keymap

# Build y flash
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_left
west flash
```

**Layout de prueba:**
```
F1   F2   F3   F4   F5
F6   F7   F8   F9   F10
F11  F12  ESC  TAB  RET
DEL  BSPC SPACE -     =
```

**Verificación:**
- Presionar esquina superior izquierda → Debe ser F1
- Presionar esquina superior derecha → Debe ser F5
- Presionar esquina inferior derecha → Debe ser =

### Método 3: USB HID Test

**En Linux:**
```bash
# Instalar evtest
sudo apt install evtest

# Ejecutar
sudo evtest /dev/input/event0

# Seleccionar dispositivo del teclado
# Presionar teclas y ver códigos
```

**Qué observar:**
- Event type: EV_KEY
- Code: número de tecla
- Value: 1 (pressed) / 0 (released)

### Método 4: UART Debug (Avanzado)

**Configuración:**
```bash
# Usar configuración de debug
west build -p -b nice_nano_v2 -- -DSHIELD=skeletyl_left \
  -DCONFIG_ZMK_UART_LOGGING=y \
  -DCONFIG_ZMK_USB_LOGGING=y
```

**Conectar UART:**
```bash
# Linux
screen /dev/ttyACM0 115200

# O usar minicom, putty, etc.
```

**Salida esperada:**
```
[KSCAN] Matrix event: row=0 col=0 pressed=1
[KSCAN] Matrix event: row=0 col=0 pressed=0
```

## Resultados Esperados

### Configuración Actual (Pro Micro Pins)

| Posición | Pin Esperado | Debe Ser |
|----------|--------------|----------|
| R1C1     | 18 (P0_03)   | F1       |
| R1C2     | 4 (P0_02)    | F2       |
| R1C3     | 5 (P1_15)    | F3       |
| R1C4     | 9 (P0_09)    | F4       |
| R1C5     | 20 (P0_28)   | F5       |
| R2C1     | 10 (P1_04)   | F6       |
| R2C2     | 6 (P1_13)    | F7       |
| R2C3     | 7 (P1_11)    | F8       |
| R2C4     | 8 (P0_10)    | F9       |
| R2C5     | 18           | F10      |

## Interpretación de Resultados

### ✅ Pin Correcto
- Presionas tecla en posición X
- ZMK Studio muestra tecla X
- → No necesitas cambiar nada

### ❌ Pin Incorrecto
- Presionas tecla en posición X
- ZMK Studio muestra tecla Y
- → Los pines están cruzados
- → Necesitas corregir en `.overlay`

### ❌ Sin Respuesta
- Presionas tecla en posición X
- No pasa nada
- → Pin mal configurado o sin conexión

## Corrección de Pines

Si encuentras errores, edita `boards/shields/skeletyl/skeletyl_left.overlay`:

```dts
&kscan0 {
    row-gpios
        = <&pro_micro 18 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> // R1
        , <&pro_micro 4 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> // R2
        , <&pro_micro 5 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> // R3
        , <&pro_micro 9 (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)> // R4
        ;
    col-gpios
        = <&pro_micro 20 (GPIO_ACTIVE_HIGH)> // C1
        , <&pro_micro 10 (GPIO_ACTIVE_HIGH)> // C2
        , <&pro_micro 6 (GPIO_ACTIVE_HIGH)> // C3
        , <&pro_micro 7 (GPIO_ACTIVE_HIGH)> // C4
        , <&pro_micro 8 (GPIO_ACTIVE_HIGH)> // C5
        ;
};
```

## Herramientas Adicionales

### ZMK GUI Mapper
Herramienta específica para mapeo de matrices:
```bash
git clone https://github.com/zmkfirmware/zmk-gui-mapper
cd zmk-gui-mapper
pip install -r requirements.txt
python zmk_gui_mapper.py
```

### QMK Toolkit (Alternativa)
Si también tienes QMK:
- Conectar teclado
- Detectar automáticamente
- Ver layout visual

## Notas

- Testear ambas mitades (left y right)
- Verificar con teclado físico y ZMK Studio
- Anotar todos los mismatches
- Corregir un error a la vez

## Siguiente Paso

Una vez verificado el mapeo:
1. Restaurar keymap original: `cp config/skeletyl.keymap.backup config/skeletyl.keymap`
2. Usar ZMK Studio para cambios finales
3. O corregir pines si hay errores
