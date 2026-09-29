# ArdECG: Real-Time Arduino Electrocardiogram & HRV Monitor

A portable, real-time medical biopotential Electrocardiogram (ECG) monitor and Heart Rate Variability (HRV) analyzer built on the Arduino platform with an ILI9341 320x240 SPI color TFT display.

The firmware executes high-speed register-level analog sampling, physiological grid rendering, dynamic R-wave peak detection, and non-linear Poincaré plot visualization.

---

## Features

- **High-Speed Deterministic Biopotential Acquisition**:
  - Direct ATmega328P ADC register control (`ADCSRA`, `ADSC`, `ADIF`) executing precise 200 Hz sampling ($T_s = 5\text{ ms}$).
  - Hardware Leads-Off detection (`LO+` and `LO-` pins) to monitor electrode disconnects in real time.
- **Multiple Visual Operating Modes**:
  1. **Full-Screen ECG (`mdLargeECG`)**: Sweeping high-resolution cardiographic trace overlaid onto calibrated 1 mm / 5 mm equivalent medical grids.
  2. **Split Screen (`mdSmallECG`)**: ECG waveform alongside heart rate (BPM) metrics and rolling statistics.
  3. **Poincaré Plot (`mdPoincare`)**: Phase-space scatter plot graphing successive RR intervals ($RR_n$ vs $RR_{n+1}$) for evaluating autonomic tone and Heart Rate Variability (HRV).
- **R-Wave QRS Peak Detection & BPM Processing**:
  - Real-time digital filtering and peak-finding algorithm to accurately isolate QRS complexes from baseline wander and muscle artifact noise.
  - Beat-to-beat interval calculation and instantaneous heart rate calculation.
- **Low-Latency Display Driver**:
  - High-performance bit-level SPI driver (`SimpleILI9341`) providing flicker-free sweep updates without full-screen buffer memory bottlenecks.

---

## Hardware Specification

### Components
- **Microcontroller**: Arduino Uno / Nano / Pro Mini (ATmega328P @ 16 MHz).
- **Analog Front-End (AFE)**: Analog Devices AD8232 (or compatible single-lead heart rate monitor front-end).
- **Display**: 2.4" / 2.8" SPI TFT LCD (ILI9341 controller, 320x240 resolution).
- **Control Input**: Mode selection pushbutton on analog pin A5.

### Pinout Mapping

| Subsystem | Arduino Pin | Description |
| :--- | :--- | :--- |
| **ECG Signal In** | A2 | Analog output from ECG front-end |
| **Leads-Off Plus (LO+)**| A0 | Digital lead disconnect detection positive |
| **Leads-Off Minus (LO-)**| A1 | Digital lead disconnect detection negative |
| **Pushbutton Mode** | A5 | Active LOW display mode switch |
| **TFT CS** | D10 | SPI Chip Select |
| **TFT CD / DC** | D8 | Command / Data select |
| **TFT Reset (RST)**| D9 | Hardware display reset |
| **TFT MOSI** | D11 | Hardware SPI Data Out |
| **TFT SCK** | D13 | Hardware SPI Clock |

---

## Project Structure

```text
ArdECG-Monitor/
├── ArdECG1.ino                       # Master Arduino firmware (acquisition, filtering, UI)
├── .gitignore                        # Git ignore rules
└── README.md                         # Project documentation
```

---

## Compilation & Usage

1. Open `ArdECG1.ino` in the Arduino IDE.
2. Ensure you have the `SPI` library available.
3. Select **Arduino Uno** (or **Arduino Nano**) under **Tools > Board**.
4. Attach ECG surface electrodes (Right Arm, Left Arm, Right Leg ground) to the AD8232 module.
5. Power on the device. Press the button on pin A5 to toggle between Large ECG sweep, Small ECG, and the Poincaré HRV scatter plot.

---

## License

Educational and bio-medical engineering prototype. Original core ECG routine based on implementation by Peter Balch.
