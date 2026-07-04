# MSXnano · Boards

**Proof-of-concept carrier boards for the [MSXnano](https://github.com/Papipapito/MSXnano) project (Sipeed Tang Nano 20K).**
**Placas portadoras de prueba de concepto para el proyecto [MSXnano](https://github.com/Papipapito/MSXnano) (Sipeed Tang Nano 20K).**

*(English below · Español debajo)*

📄 Rich readme / readme enriquecido: [`README.html`](README.html) · 🖼️ Renders: [assembled](dock20k/render_assembled.png) · [board](dock20k/render_dock.png)

---

## 🇬🇧 English

These are **proof-of-concept boards built around the Sipeed M0S (BL616)** module. They exist for one exact reason:

> Since **~February 2024**, Sipeed ships Tang Nano 20K boards (silk-marked **`3921`**) whose **on-board BL616** has its **secure-boot eFuse in a state that often prevents it from running the FPGA-Companion firmware**. When that happens, the **USB keyboard and mouse stop working** (HDMI and the on-screen menu still work — that part is the FPGA, not the BL616). Sipeed's official workaround is to use an **external M0S Dock**.

Instead of hanging an external dock off the board with loose wires, these boards **integrate a dedicated M0S (BL616)** as the USB companion — plus a USB hub and WiFi — onto a single carrier, so **everything runs through the Tang's 2×20 headers with no external cables**.

### Boards

| Board | What it is | Status |
|-------|------------|--------|
| [**dock20k**](dock20k/) | Tang Nano 20K carrier: **M0S (BL616)** companion + **FE1.1s** 4-port USB hub (2×USB-A + 2×USB-C) + **ESP32-C6** WiFi (UNAPI). Single USB-C power in. | Schematic ERC 0 · PCB routed (DRC 0) · **not yet fabricated** |

### dock20k highlights

- **M0S (BL616)** runs the stock [FPGA-Companion](https://github.com/MiSTle-Dev/FPGA-Companion) `M0S_DOCK` build over the external `m0s[]` SPI path — **no RTL changes** to the MSXnano core.
- **ESP32-C6-WROOM-1-N8** for internet, running ducasp's [ESP32-UNAPI firmware](https://github.com/ducasp/ESP32-UNAPI-Firmware). It's the C6 (not the classic WROOM) because the core fixes the WiFi UART at **859372 bps**, a rate that is unreliable on the WROOM but fine on the C6.
- Everything routed through the **Tang's own 2×20 headers** — no external cables. Free Tang pins are broken out on an expansion header for future add-ons (e.g. MSX-MIDI).
- Connector & jumper reference: [`dock20k/CONNECTORS.md`](dock20k/CONNECTORS.md).

> ⚠️ **Proof of concept.** Designed and routed in KiCad but **not manufactured or tested in hardware yet.** Before ordering, verify the Tang socket against a physical board (rows at 20.32 mm, pin-1 position).

---

## 🇪🇸 Español

Estas son **placas de prueba de concepto construidas alrededor del módulo Sipeed M0S (BL616)**. Existen por una razón exacta:

> Desde **~febrero de 2024**, Sipeed fabrica placas Tang Nano 20K (marcadas **`3921`** en la serigrafía) cuyo **BL616 de a bordo** tiene el eFuse de **secure-boot en un estado que a menudo le impide ejecutar el firmware FPGA-Companion**. Cuando eso pasa, **el teclado y el ratón USB dejan de funcionar** (el HDMI y el menú en pantalla sí van — eso lo hace la FPGA, no el BL616). La solución oficial de Sipeed es usar un **M0S Dock externo**.

En lugar de colgar un dock externo con cables sueltos, estas placas **integran un M0S (BL616) dedicado** como companion USB —además de un hub USB y WiFi— en una sola portadora, de modo que **todo funciona por los conectores 2×20 del Tang, sin cables externos**.

### Placas

| Placa | Qué es | Estado |
|-------|--------|--------|
| [**dock20k**](dock20k/) | Portadora del Tang Nano 20K: companion **M0S (BL616)** + hub USB **FE1.1s** de 4 puertos (2×USB-A + 2×USB-C) + WiFi **ESP32-C6** (UNAPI). Entrada única de 5 V por USB-C. | Esquemático ERC 0 · PCB rutada (DRC 0) · **sin fabricar aún** |

### Claves de dock20k

- El **M0S (BL616)** ejecuta el build `M0S_DOCK` de [FPGA-Companion](https://github.com/MiSTle-Dev/FPGA-Companion) tal cual, por la ruta SPI externa `m0s[]` — **sin tocar el RTL** del core MSXnano.
- **ESP32-C6-WROOM-1-N8** para internet, con el [firmware ESP32-UNAPI](https://github.com/ducasp/ESP32-UNAPI-Firmware) de ducasp. Es el C6 (no el WROOM clásico) porque el core fija la UART del WiFi a **859372 bps**, una velocidad poco fiable en el WROOM pero correcta en el C6.
- Todo enrutado por los **conectores 2×20 del propio Tang** — sin cables externos. Los pines libres del Tang salen a un cabezal de expansión para futuras ampliaciones (p. ej. MSX-MIDI).
- Referencia de conectores y jumpers: [`dock20k/CONNECTORS.md`](dock20k/CONNECTORS.md).

> ⚠️ **Prueba de concepto.** Diseñada y rutada en KiCad pero **aún no fabricada ni probada en hardware.** Antes de encargarla, verifica el zócalo del Tang contra una placa física (filas a 20.32 mm, posición del pin 1).

---

### Credits / Créditos

Board design: **Papipapito**. Third-party footprints/3D models (Sipeed M0S, Espressif ESP32-C6) keep their own licenses. BL616 firmware: [FPGA-Companion](https://github.com/MiSTle-Dev/FPGA-Companion) (MiSTle-Dev). WiFi firmware: [ESP32-UNAPI](https://github.com/ducasp/ESP32-UNAPI-Firmware) (ducasp).

*Diseño de la placa: **Papipapito**. Las huellas/modelos 3D de terceros conservan su licencia. Firmware BL616: FPGA-Companion. Firmware WiFi: ESP32-UNAPI de ducasp.*
