# Wave Digital Filter Model of the MXR Distortion+ Diode Clipper

Virtual analog model of the diode-clipper stage of the **MXR Distortion+** guitar pedal, implemented in the **Wave Digital Filter (WDF)** domain in MATLAB and validated against a Simscape circuit simulation.

Homework 2 for the course *Sound Analysis, Synthesis and Processing*, MSc in Music and Acoustic Engineering, Politecnico di Milano (a.y. 2023/2024).
Authors: Mattia Montanari, Flavio Ingenito.

## What it does

The analog reference circuit is converted into a WDF structure and simulated sample by sample at **96 kHz**. The input can be a 440 Hz sine wave or a real guitar recording (`guitar_input.wav`, resampled from 44.1 kHz).

## Model

- **Linear part:** input resistance, R1, Rout, C1 and C2, connected through two series and two parallel 3-port adaptors. Port resistances are chosen so the adaptors are reflection-free, and the scattering matrices are computed from the series and parallel junction equations.
- **Capacitors:** discretized with the trapezoidal rule (port resistance Ts/2C).
- **Nonlinearity:** a pair of antiparallel **1N914** diodes (Shockley model, with saturation current, ideality factor and thermal voltage). The scattering relation is solved **explicitly with the Lambert W function** (Fritsch approximation), so no iterative solver runs in the sample loop.
- **Algorithm:** forward scan from the leaves to the nonlinear root, explicit diode reflection, then backward scan. The output voltage is read from the waves at the output resistor.

## Validation

`diode_clipper_WD.m` plots the WDF output against the Simscape ground truth (`diode_clipper_SSC.slx` → `ssc_output.mat`), together with the error signal over five periods of the test tone.

<!-- Add here: the output and error plots (PNG), and one or two sentences on the result, taken from report.pdf -->

## Files

| File | Content |
|---|---|
| `diode_clipper_WD.m` | Main script: WDF setup, simulation loop, plots |
| `antiparallel_diodes.m` | Explicit scattering function of the antiparallel diode pair |
| `Lambert_W_Fritsch.m` | Lambert W approximation (Fritsch iteration) |
| `diode_clipper_SSC.slx` | Simscape reference circuit |
| `ssc_output.mat` | Simscape output used as ground truth |
| `guitar_input.wav` | Guitar test signal |
| `report.pdf` | Full report |

## How to run

Open MATLAB in the repository folder and run `diode_clipper_WD.m`. Uncomment the "Music Signal Test" block to process the guitar recording, and `sound(vout, fs)` to listen to the result.

