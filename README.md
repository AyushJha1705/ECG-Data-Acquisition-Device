# PCB Prototype of ECG Data Acquisition System

## Overview
This repository contains the simulation files, schematic designs, and PCB fabrication files (Gerbers) for a robust, low-power Electrocardiogram (ECG) Data Acquisition (DAQ) system. The project was developed to accurately extract microvolt-level (0.5 mV to 5 mV) ECG signals from noisy environments and amplify them for analog-to-digital conversion (ADC).

## Hardware Architecture
The Analog Front-End (AFE) utilizes a multi-stage filtering and amplification process to isolate the ECG signal from biological and environmental interference:

*   **Instrumentation Amplifier (InAmp):** Uses an AD620 IC to provide a high Common-Mode Rejection Ratio (CMRR) of over 100 dB. This stage extracts the raw ECG signal while actively filtering out 50Hz power-line interference.
*   **High Pass Filter (HPF):** Implemented using a TL074 JFET-input Quad Op-Amp in a 2nd order Sallen-Key topology. With a cutoff frequency set at 0.01 Hz, this stage removes severe DC offsets (up to 300mV) caused by the skin-electrode interface and mitigates baseline wander from patient respiration.
*   **Gain Stage:** Utilizes another TL074 op-amp to amplify the signal by a factor of 15, ensuring the waveform reaches an ADC-processable magnitude without rail saturation.
*   **Low Pass Filter (LPF):** A 6th order Butterworth filter (Sallen-Key topology) with a sharp 120 dB/decade roll-off and a cutoff frequency of 75 Hz. This aggressively filters out high-frequency electromyographic (EMG) muscle noise and electronic interference.
*   **Total System Gain:** The staggered gain across all stages (InAmp: 5, HPF: 1.586, Gain Stage: 15, LPF: 4.22) yields a net system gain of 502, safely scaling the 1mV signal to an ADC-compatible range centered on a 2V offset.

## Simulation & Validation
The circuit's functional integrity was rigorously simulated and verified using MicroCap software. 
*   **Datasets Used:** Tested against real-world clinical data from the MIT-BIH Arrhythmia Database (Datasets 100, 101) and the MIT-BIH Normal Sinus Rhythm Database (Datasets 16265, 16273).
*   **Noise Rejection:** Simulations confirm the successful rejection of introduced 150 Hz and 200 Hz sinusoidal noise, as well as the complete blocking of 300mV DC offsets without phase distortion or QRS complex degradation.

## PCB Layout & Fabrication
The physical prototype was designed and routed using KiCad, focusing on spatial optimization and noise rejection.
*   **Board Dimensions:** Compact 2-layer design measuring 43.5 mm x 75.5 mm.
*   **Routing Protocols:** Trace widths were set to 0.8 mm for power lines and 0.2 mm for signals. The board utilizes 45-degree bend routing to minimize signal integrity issues and reduce electromagnetic interference (EMI).
*   **Grounding & Bypassing:** The entire bottom layer (B.Cu) serves as a dedicated ground plane to provide a low-impedance return path. Unpolarized 0.1uF ceramic bypass capacitors are placed adjacent to all power pins to suppress high-frequency AC noise and prevent oscillation during fast switching transitions.
