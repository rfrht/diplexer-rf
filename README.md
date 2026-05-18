# Zamin ABL1000 40m Absorptive Diplexer (Rev. A)

> A high-power, 40m (7 MHz) absorptive diplexer designed to clean the output of a Zamin ABL1000 solid-state amplifier and safely dissipate destructive out-of-band harmonics.

## 📌 Project Objective
The Zamin ABL1000 utilizes four M25C24 bipolar transistors in a push-pull configuration. When driven to its absolute 600W limit, it operates deep into saturation, effectively acting as a square-wave generator rich in **odd-order harmonics** (especially the 3rd harmonic at 21 MHz). 

Standard low-pass filters reflect these harmonics back into the amplifier finals, causing extreme thermal stress and potential failure. This **absorptive diplexer** solves the problem by passing the clean 7 MHz fundamental frequency to the antenna while routing the destructive 14 MHz, 21 MHz, and 28 MHz+ harmonics into a dedicated 50-Ohm dummy load.

## 📐 Filter Topology & Specifications
* **Architecture:** 7.5 MHz Crossover, 3-Pole Butterworth Contiguous Diplexer.
* **System Impedance:** 50 Ohms constant (ensuring a flat 1.0:1 SWR presented to the amplifier across the spectrum).
* **Schematic:** [Check here](https://github.com/rfrht/diplexer-rf/blob/main/Others/lpf-40-sch.png).

### Branch Values
| Branch | Port | Component | Target Value | Actual Assembly Value |
| :--- | :--- | :--- | :--- | :--- |
| **Low-Pass** | Antenna | L1 (Series) | 1.06 µH | 1.06 µH |
| **Low-Pass** | Antenna | C1 (Shunt) | 848 pF | 840 pF |
| **Low-Pass** | Antenna | L2 (Series) | 1.06 µH | 1.06 µH |
| **High-Pass** | Dummy Load | C2 (Series) | 424 pF | 420 pF |
| **High-Pass** | Dummy Load | L3 (Shunt) | 0.53 µH | 0.53 µH |
| **High-Pass** | Dummy Load | C3 (Series) | 424 pF | 420 pF |

---

## 🛠️ Hardware & Components

### 1. Capacitor Banks (1260V Series-Parallel Matrix)
To handle 500V+ RF peaks and distribute heat, standard **Yageo 1206 630V C0G ceramic capacitors** are arranged in a 2x4 matrix (2 in series, 4 parallel rows).
* **C1 Bank (840 pF):** Three rows of 470 pF + One row of 270 pF.
* **C2 & C3 Banks (420 pF):** Three rows of 220 pF + One row of 180 pF.

### 2. High-Power Air Core Inductors
* **Material:** 12 AWG bare/enameled copper wire wound on a 1-inch (25.4 mm) form.
* **L1 & L2:** ~9 turns, spaced to ~3.6 cm in length.
* **L3:** ~6 turns, spaced to ~2.54 cm in length.
* **Orthogonal Placement:** L1 is mounted horizontally (X-axis), L2 is horizontal (Y-axis), and L3 is mounted **vertically** (Z-axis). This strict 90-degree geometry guarantees zero magnetic cross-coupling.

### 3. Ports & Dummy Load
* **Main RF Ports (Input & Antenna):** High-temperature, 2,500W-rated **RG-400 Teflon coax pigtails**, soldered directly to the board and terminating in SO-239 connectors to decouple mechanical torque from the FR4 pads.
* **Dummy Load Port:** Edge-mounted PTFE SMA connector feeding a flexible RG-316 jumper.
* **DIY Dummy Load (200W):** Two parallel Ohmite TGHD SOT-227 100-Ohm thick-film resistors (yielding a 50-Ohm load) bolted to a large heatsink with thermal paste, bridged with flat copper strap to eliminate parasitic inductance.

---

## 📏 PCB Layout & RF Design Rules

[The board](https://github.com/rfrht/diplexer-rf/blob/main/Others/lpf-40.png) is a 10x10 cm, 1 oz FR4 prototype with strict RF rules applied:

* **Trace Architecture:** The main LPF signal path uses heavy **6 mm wide traces**, mirrored identically on the top and bottom layers to act as a 2 oz equivalent.
* **Via Stitching:** Top and bottom traces are tied together using vias spaced precisely every **5 mm along the trace borders** to combat the skin effect and create a highly efficient thermal superhighway. 
* **Ground Plane:** Solid copper pours on top and bottom, featuring a 5 mm checkered/staggered via grid. Dense via clusters are placed immediately at the ground pads of C1, L3, and the coax shields. Isolation gaps are set to 3-5 mm to prevent 500V+ arc-overs.
* **Solder Mask Management:**
  * *Removed* (`tStop`/`bStop`) along the main 6 mm LPF path to allow for heavy "solder river" tinning to handle 6A+ RF peaks.
  * *Retained* on the 2 mm traces between the series SMD capacitors to act as a solder dam during assembly.
* **Magnetic Keep-Out Zones:** Ground pours are **completely removed** (`tRestrict`/`bRestrict`) underneath and extending slightly outward from L1, L2, and L3 on both layers to prevent eddy currents from destroying the inductors' Q-factor.

---

## 🔬 Assembly & Testing Strategy

1. **Component Verification:** Capacitors and inductors are verified using a 100 kHz LCR bridge (FNIRSI LC1020E) to ensure strict adherence to target values and Q-factor. L3 is tuned with its physical vertical mounting lead lengths accounted for.
2. **Finding the Amplifier "Sweet Spot":** The Zamin ABL1000 will not be pushed to a flat-topped 600W. Using an FT-991A exciter and an inline power meter on the diplexer's SMA dummy load port, the drive will be slowly increased to map the "saturation knee." The final operating point will be set just before the dummy load meter spikes, yielding a clean 200W–250W output with minimal harmonic generation.

## ⚠️ Safety Warnings
* **RF Burns:** This board operates at high RF voltages and handles massive currents. Rev A is an exposed prototype. **KEEP AWAY from the board while transmitting.** 
* **ESD Mats:** Do not test this board directly on a conductive/dissipative ESD mat, as high RF voltages can cause the mat to act as a short circuit across the bottom vias, altering stray capacitance and causing melting/fires. Elevate the board on non-conductive standoffs.
