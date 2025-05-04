# FFTForge

Real‐time audio spectrum analysis on Cortex-M4, powered by FreeRTOS.

---

## Overview

**FFTForge** is a turnkey pipeline for capturing, transforming, and streaming audio data on an ARM Cortex-M4 MCU. Built on FreeRTOS and CMSIS-DSP, it delivers robust, low-latency FFT analysis—ideal for spectrum analyzers, voice-activated controls, machinery monitoring, and more.

---

## Features

- **Zero-polling Audio Capture**  
  • INMP441 MEMS microphone via I²S + DMA  
  • Ping-pong circular buffer for continuous acquisition  
- **High-Performance FFT**  
  • `arm_cfft_f32` CMSIS-DSP routine on M4 FPU  
  • 256-sample frames → 128 spectral bins  
  • Hann window for minimal leakage  
- **Dual UART Streaming**  
  • UART2: raw PCM audio  
  • UART3: FFT magnitude spectra  
  • All transfers via DMA—CPU offloads data movement  
- **RTOS-driven Determinism**  
  • Preemptive AudioCapture, FFTTask, UART Tx tasks  
  • Guaranteed deadlines, zero data loss  

---

---

## Prerequisites

- **Hardware**  
  - STM32 (e.g., F446RE) or any Cortex-M4 MCU with I²S + DMA  
  - INMP441 MEMS microphone  
- **Software**  
  - STM32CubeMX / HAL libraries  
  - FreeRTOS v10+  
  - CMSIS-DSP v5+  
  - MATLAB (for live-plot demos)  
  - Arduino IDE (optional bridge)

---



▶️ Usage
Power up your board with the INMP441 connected.

Launch a serial terminal at 115200 baud for UART2 (raw PCM) and UART3 (FFT).

Run the MATLAB script in /MATLAB/live_plot.m to visualize raw vs. processed data in real time.

(Optional) Use an Arduino Uno as a USB‐serial bridge:

RX/TX pins forward UART2/3 to PC

Live plot or custom host application can subscribe to either stream.

Architecture & Tasks
Task Name	Priority	Role
AudioCapture	High	I²S + DMA fills ping-pong buffer, signals FFTTask
FFTTask	Medium	Applies Hann window, runs CMSIS-DSP FFT, signals UARTTx
UART2_Tx_Task	Low	Streams raw PCM frames
UART3_Tx_Task	Low	Streams FFT magnitude bins

MATLAB Demo
matlab
Copy
Edit
% live_plot.m
% - Opens two serial ports
% - Reads PCM frames & FFT bins
% - Displays scrolling plots side-by-side

Contributing
Fork the repo

Open a pull request – happy to review schematics, test results, or optimization ideas!

Questions? Open an issue or DM me for schematics, code snippets, or an RTOS deep-dive.
