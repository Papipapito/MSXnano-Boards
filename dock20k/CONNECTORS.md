# dock20k — Connectors / Conectores (J1–J12)

Reference for every connector and jumper: what it is, its pinout and what to use it for.
Reader-friendly HTML version: [`../README.html`](../README.html).

Referencia de todos los conectores y jumpers: qué son, su pinout y para qué usarlos.
Versión HTML más cómoda de leer: [`../README.html`](../README.html).

> Pin numbers match the PCB silkscreen (pin 1 has a square pad). On 2-row connectors,
> the odd column is 1/3/5… and the even column is 2/4/6….
> **[cap]** = a 2.54 mm jumper. "Bridge A-B" = place a cap over pins A and B.
>
> Los números de pin coinciden con la serigrafía (el pin 1 lleva pad cuadrado). En los
> conectores de 2 filas, la columna impar es 1/3/5… y la par 2/4/6….
> **[cap]** = puente de 2.54 mm. "Puentear A-B" = colocar un cap sobre los pines A y B.

---

## Summary / Resumen

| Ref | Silk | Type / Tipo | Use / Uso | Jumper? |
|-----|------|-------------|-----------|---------|
| **J1**  | USB-C_POWER   | USB-C recept. | 5 V power in / entrada 5 V (+ BL616 flash data) | — |
| **J2**  | ESP_FLASH     | 1×6 header    | Program the ESP32-C6 / programar el ESP32-C6 | — |
| **J3**  | ESP_UART_LINK | 2×2           | Connect/isolate FPGA↔ESP UART / conectar-aislar UART | **Yes (2)** |
| **J4**  | USB-A_1       | USB-A recept. | USB output port / puerto USB de salida | — |
| **J5**  | USB-A_2       | USB-A recept. | USB output port / puerto USB de salida | — |
| **J6**  | USB-C_1       | USB-C recept. | USB output port / puerto USB de salida | — |
| **J7**  | USB-C_2       | USB-C recept. | USB output port / puerto USB de salida | — |
| **J8**  | WS2812_STRIP  | JST-XH 1×3    | WS2812 LED strip / tira de LEDs WS2812 | — |
| **J9**  | BL616_FLASH   | 1×4 header    | BL616 serial log + emergency flash | — |
| **J10** | BL_USB_SEL    | 2×3           | Route BL616 USB: hub vs flash / enrutar USB del BL616 | **Yes (2)** |
| **J11** | EXPANSION     | 2×6           | Free Tang pins + power / pines libres del Tang | — |
| **J12** | TangNano20K   | 2×20 socket   | Where the Tang plugs in / donde se enchufa el Tang | — |

Buttons / Pulsadores: **SW1** = ESP reset, **SW2** = ESP boot, **SW3** = BL616 boot, **SW4** = BL616 reset.

---

## The two config jumpers / Los dos jumpers de configuración

### J3 — ESP_UART_LINK (FPGA ↔ ESP32 serial link / enlace serie)

```
 row1   1 FPGA_TX  ──  2 ESP_RX0     [cap 1-2]
 row2   3 ESP_TX0  ──  4 FPGA_RX     [cap 3-4]
```

- **EN — Normal (WiFi working):** both caps on (1-2 and 3-4). ESP and core talk at 859372 bps.
  **To flash the ESP:** remove both caps, so the FPGA lines don't fight the USB-serial adapter on J2.
- **ES — Normal (WiFi funcionando):** los dos caps puestos (1-2 y 3-4). ESP y core hablan a 859372 bps.
  **Para flashear el ESP:** quita los dos caps, para que las líneas de la FPGA no peleen con el adaptador USB-serie de J2.

### J10 — BL_USB_SEL (where the BL616's single USB goes / a dónde va el único USB del BL616)

```
          D+ column        D- column
 row1   1 HUB_UP_DP      2 HUB_UP_DM     → to the USB hub (normal / normal)
 row2   3 BL_USB_DP      4 BL_USB_DM     → BL616 USB (common / común)
 row3   5 PC_DP          6 PC_DM         → J1 data lines (flashing / flasheo)
```

- **EN — Normal (keyboard/mouse via hub):** caps on **1-3 and 2-4** (BL616 → hub). Default position.
  **To flash the BL616:** move caps to **3-5 and 4-6** (BL616 → J1), then plug a USB-C **data** cable from J1 to the PC.
- **ES — Normal (teclado/ratón por el hub):** caps en **1-3 y 2-4** (BL616 → hub). Posición por defecto.
  **Para flashear el BL616:** mueve los caps a **3-5 y 4-6** (BL616 → J1), y conecta un cable USB-C **de datos** de J1 al PC.

---

## Programming headers / Cabeceras de programación

### J2 — ESP_FLASH (1×6) — program the ESP32-C6 / programar el ESP32-C6

```
 1  +3V3   ESP logic (don't connect if already USB-C powered) / lógica del ESP (no conectar si ya alimentas por USB-C)
 2  GND
 3  EN     ESP reset  → adapter RTS (or SW1) / reset del ESP → RTS (o SW1)
 4  BOOT   GPIO9      → adapter DTR (or SW2) / arranque → DTR (o SW2)
 5  TX0    GPIO16     → adapter RX
 6  RX0    GPIO17     → adapter TX
```
Remove J3 caps before flashing here. / Quita los caps de J3 antes de flashear por aquí.
After the first flash, the firmware updates over WiFi (OTA) or via the MSX link.
Tras el primer flasheo, el firmware se actualiza por WiFi (OTA) o por el enlace MSX.

### J9 — BL616_FLASH (1×4) — log + emergency flash / log + flasheo de emergencia

```
 1  +3V3
 2  GND
 3  TX     BL616 GPIO21  → adapter RX   (serial log 115200 8N1 / log serie 115200 8N1)
 4  RX     BL616 GPIO22  → adapter TX
```

---

## Power & outputs / Alimentación y salidas

- **J1 — USB-C_POWER.** EN: powers the whole board and the Tang (via its 5 V pin, through an SS34 blocking diode); its data lines feed J10 (used only when flashing the BL616). · ES: alimenta toda la placa y el Tang (por su pin de 5 V, con diodo SS34 anti-retorno); sus líneas de datos van a J10 (solo se usan al flashear el BL616).
- **J4 / J5 (USB-A) · J6 / J7 (USB-C).** EN: the four USB output ports of the FE1.1s hub — keyboard, mouse, joypad, flash drive. Each has a 500 mA polyfuse + ESD. USB-C ports act as host (56 k Rp). · ES: los cuatro puertos USB de salida del hub FE1.1s — teclado, ratón, joypad, pendrive. Cada uno con polyfuse de 500 mA + ESD. Los USB-C actúan como host (Rp 56 k).
- **J8 — WS2812_STRIP (JST 1×3):** `1=+5V, 2=DIN (Tang pin 25 via 330 Ω), 3=GND`. EN: addressable LED strip (case). · ES: tira de LEDs direccionables (caja).

### J11 — EXPANSION (2×6) — free Tang pins / pines libres del Tang

```
 odd column (left)      even column (right)
 1  3V3 (Tang)          2  +5V
 3  P73                 4  P74
 5  P77                 6  P29
 7  P30                 8  P31
 9  P72                10  P71
11  GND                12  GND
```
EN: unused FPGA balls + power, for future add-ons (e.g. MSX-MIDI) without redesigning the board. Assign a new signal in the core's `.cst` to one of these pins.
ES: balls de la FPGA sin usar + alimentación, para ampliaciones futuras (p. ej. MSX-MIDI) sin rediseñar la placa. Asigna la señal nueva en el `.cst` del core a uno de estos pines.

### J12 — TangNano20K socket / zócalo

EN: not a jumper — the 2×20 **female socket where the Tang Nano 20K plugs in**. Every signal between board and Tang passes through here (no loose wires). Rows 20.32 mm apart; row A is the 5 V side (per official 3921 schematic).
ES: no es un jumper — el **zócalo 2×20 hembra donde se enchufa el Tang Nano 20K**. Todas las señales entre placa y Tang pasan por aquí (sin cables sueltos). Filas a 20.32 mm; la fila A es la del pin de 5 V (según el esquemático oficial 3921).

---

## Recipes / Recetas

**1) Normal operation / Funcionamiento normal** — J3: both caps (1-2, 3-4) · J10: caps up (1-3, 2-4) · Tang in J12, power on J1, peripherals on J4–J7.

**2) Flash the ESP32-C6 / Flashear el ESP32-C6** — remove J3 caps · USB-UART on J2 (TX↔RX crossed) · hold **SW2 BOOT** + press **SW1 RST** · program, then restore J3 caps.

**3) Flash the BL616 / Flashear el BL616** — J10 caps to 3-5, 4-6 · USB-C data cable J1→PC · hold **SW3 BL_BOOT** + press **SW4 BL_RST** · load `fpga_companion_m0sdock.bin`, then restore J10 caps. (Alt: UART on J9.)
