
# Robot de Fútbol RC - Liga Nacional de Robótica (LNR)
> **Desarrollado para la categoría de Fútbol RC - UTN.BA**

<img width="750" height="605" alt="image" src="https://github.com/user-attachments/assets/92470e88-64d1-4c79-a722-d3c9450df961" />

Este repositorio contiene el diseño mecánico, esquemáticos de electrónica, PCB y firmware para el robot de fútbol radiocontrolado (RC) desarrollado en la **Universidad Tecnológica Nacional, Facultad Regional Buenos Aires (UTN.BA)**, concebido para competir en la **Liga Nacional de Robótica (LNR) de Argentina**.

---

## 📋 Características Principales

* **Microcontrolador:** ESP32-C3 SuperMini (SoC RISC-V de 32 bits con conectividad Wi-Fi / Bluetooth LE integrados).
* **Control Remoto:** Control en tiempo real mediante comunicación **Bluetooth LE (BLE)** integrado en el ESP32.
* **Manejo de Motores / Driver:** Controlador dual **L9110** montado sobre PCB personalizada de 60 mm x 45 mm.
* **Sistema de Tracción:** 
  * Tracción diferencial de 2 ruedas impulsadas por motoreductores amarillos clásicos con caja reductora integrados a ruedas de gran agarre.
  * Apoyo frontal mediante rueda loca / Caster (`mini-Porta-Rueda-Crazy`).
* **Estructura y Chasis:** 
  * Chasis modular impreso en **PLA** (`CUERPO_BOT` y `CUERPO_TOP`).
  * Guía/Pala frontal en forma de "V" (`PALA`) optimizada para guiado y control del balón.
  * Distribución de pesos balanceada con lastre de plomo (`plomo`) para reducir el centro de gravedad e incrementar la tracción en pista.
* **Alimentación:**
  * 2 celdas Li-Ion 18650 en configuración 2S (7.4V nominal / 8.4V máx).
  * Portabaterías modular integrado al chasis (`battery-holder-18650x2`).

---

## 🛠️ Estructura del Diseño 3D (CAD)

El modelado 3D fue realizado en **SolidWorks**, dividiéndose en los siguientes componentes imprimibles y de ensamble:

| Componente | Archivo CAD / Pieza | Descripción |
| :--- | :--- | :--- |
| **Chasis Principal** | `CUERPO_BOT` | Estructura base que soporta motores, PCB y baterías. |
| **Cubierta Superior** | `CUERPO_TOP` | Tapa protectora con relieve / Isologo grabado. |
| **Guía de Pelota** | `PALA` | Pala frontal en V para dirección y control de la pelota. |
| **Rueda Caster** | `mini-Porta-Rueda-Crazy` | Soporte para rueda loca / de apoyo frontal. |
| **Placa Electrónica** | `PCB-60x45` | PCB dedicada para alimentación, driver L9110 y ESP32-C3. |
| **Lastre** | `plomo` | Pesos integrados para mejorar el agarre de las ruedas. |

---

## 🔌 Electrónica y PCB (60 mm x 45 mm)

La placa principal fue diseñada a medida e integra los siguientes componentes y puertos de expansión:

<img width="1196" height="390" alt="image" src="https://github.com/user-attachments/assets/d049a04a-4e17-47a7-936d-22fe07ba59f6" />


* **Control e Inteligencia:** Zócalo para **ESP32-C3 SuperMini**.
* **Driver de Motores:** Integrado **L9110** (`U2`) de 2 canales.
  * Mapeo de control: `A1`, `A2` (Canal A / Motor Izquierdo) y `B1`, `B2` (Canal B / Motor Derecho).
* **Alimentación y Seguridad:**
  * **`J1` (Entrada Tensión):** Conexión principal de batería (`Vin_BAT` / `GND`).
  * **`JP1` / `JP2`:** Jumpers de emergencia / corte y puente de alimentación.
* **Puertos de Expansión:**
  * **`J5` (Conector I2C):** Salida I2C (`SDA`, `SCL`, `GND`, `VCC`) con resistencias pull-up `R2` y `R3` de 10k$\Omega$.
  * **`J9` (Conector Serie):** UART para depuración o sensores (`RX`, `TX`, `GND`, `VCC`).
  * **`J4` (Salida PWM):** Salida de control PWM general (`PWM0`, `VCC`, `GND`).
  * **`J8` (Salida Auxiliar):** Puerto de alimentación directo (`VCC`, `GND`).
* **Interfaz y Periféricos:**
  * **`J7` / `BUT`:** Botón configurable de usuario con resistencia de pull-up.
  * **`J6` / `LED`:** LED indicador de estado con resistencia limitadora `R1`.
  * **Filtrado:** Capacitores de desacople `C1` ($0.1\,\mu\text{F}$) y `C2` ($10\,\mu\text{F}$).

### Diagrama de Bloques

```text
                +----------------------------+
                |    2x Baterías Li-Ion      |
                |    18650 (7.4V - 8.4V)     |
                +--------------+-------------+
                               |
                               v (J1 / Vin_BAT)
                       +---------------+
                       | PCB Principal |
                       |  (60x45 mm)   |
                       +-------+-------+
                               |
        +----------------------+----------------------+
        |                                             |
        v                                             v
+---------------+                             +---------------+
| ESP32-C3      |---(A1, A2, B1, B2 PWM)----->| Driver Dual   |
| SuperMini     |                             | L9110         |
+-------+-------+                             +-------+-------+
        ^                                             |
        | (Bluetooth LE)                              | (Motor A / Motor B)
        v                                             v
+---------------+                             +---------------+
| Dispositivo   |                             | 2x Motores    |
| Control (App) |                             | Amarillos TT  |
+---------------+                             +---------------+
