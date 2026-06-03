# 2-Stage Miller-Compensated OTA — Design Assignment 2

**Name:** Chobe Atharwa Atul  
**Roll No.:** 240102106  
**Technology:** TSMC 180nm (tsmc018.lib)

---

## Overview

This project contains the hand design, LTSpice simulations, and analysis of a two-stage Miller-compensated Operational Transconductance Amplifier (OTA) in TSMC 180nm CMOS technology. The OTA is configured as a non-inverting amplifier with a closed-loop gain of 2.

---

## Technology Parameters

| Parameter | Value |
|-----------|-------|
| V_tn | 0.37 V |
| V_tp | 0.39 V |
| μnCox | 230 μA/V² |
| μpCox | 100 μA/V² |
| VDD | 1.8 V |
| L_min | 0.18 μm |
| W_min | 0.27 μm |

**Design assumptions:** λp = λn = 0.1, CL = 5 pF, VOV = 0.2 V for all transistors, L = 0.5 μm for all transistors.

---

## Circuit Topology

A standard two-stage Miller OTA with the following transistor assignments:

| Transistor | Role |
|------------|------|
| M0 | Tail current source (input stage) |
| M1, M2 | Input differential pair (NMOS) |
| M3, M4 | Active load current mirror (PMOS) |
| M5 | Second stage amplifier (PMOS) |
| M6 | Second stage current source (NMOS) |
| M7 | Bias generation (current mirror) |

Miller compensation network: capacitor **C = 2 pF** in series with nulling resistor **Rz = 217.39 Ω** to eliminate the RHP zero.

---

## Hand-Calculated Design Parameters

| MOS | W/L | W | L | gm | r_o | VGS−Vth | ID |
|-----|-----|---|---|----|-----|----------|----|
| M0 | 20 | 10 μm | 0.5 μm | 920 μΩ⁻¹ | 0.1087 MΩ | 0.2 V | 92 μA |
| M1 | 10 | 5 μm | 0.5 μm | 460 μΩ⁻¹ | 0.2173 MΩ | 0.2 V | 46 μA |
| M2 | 10 | 5 μm | 0.5 μm | 460 μΩ⁻¹ | 0.2173 MΩ | 0.2 V | 46 μA |
| M3 | 23 | 11.5 μm | 0.5 μm | 460 μΩ⁻¹ | 0.2173 MΩ | 0.2 V | 46 μA |
| M4 | 23 | 11.5 μm | 0.5 μm | 460 μΩ⁻¹ | 0.2173 MΩ | 0.2 V | 46 μA |
| M5 | 230 | 115 μm | 0.5 μm | 4600 μΩ⁻¹ | 21.73 kΩ | 0.2 V | 460 μA |
| M6 | 100 | 50 μm | 0.5 μm | 4600 μΩ⁻¹ | 21.73 kΩ | 0.2 V | 460 μA |
| M7 | 2 | 1 μm | 0.5 μm | 92 μΩ⁻¹ | 1.087 MΩ | 0.2 V | 9.2 μA |

**Reference current:** Iref = 9.2 μA (= 1/10 of tail current I0 = 92 μA)

---

## Gain Analysis

| Stage | Expression | Value |
|-------|-----------|-------|
| Stage 1 | Av1 = −gm1 · (ro2 ‖ ro4) | −50 |
| Stage 2 | Av2 = −gm5 · (ro5 ‖ ro6) | −50 |
| **Overall DC Gain** | Av = Av1 × Av2 | **2500 (≈ 67.95 dB)** |

---

## Frequency Compensation

| Parameter | Value |
|-----------|-------|
| Compensation capacitor C | 2 pF |
| Nulling resistor Rz | 217.39 Ω |
| Dominant pole p1 | −92 krad/s |
| Second pole p2 | −920 Mrad/s |
| Third pole p3 (with Rz) | −2300 Mrad/s |
| GBW (ωgc = gm1/C) | 230 Mrad/s |
| **Phase Margin (hand calc)** | **70.25°** |

---

## Simulated Results (LTSpice / TSMC 180nm)

### Transistor Operating Points

| MOS | gm | r_o | VGS−Vth | ID |
|-----|-----|-----|----------|----|
| M0 | 819 μΩ⁻¹ | 80.9 kΩ | 0.178 V | 78.1 μA |
| M1 | 435 μΩ⁻¹ | 273.2 kΩ | 0.166 V | 39.1 μA |
| M2 | 435 μΩ⁻¹ | 273.2 kΩ | 0.166 V | 39.1 μA |
| M3 | 324 μΩ⁻¹ | 256.4 kΩ | 0.226 V | 39.1 μA |
| M4 | 324 μΩ⁻¹ | 256.4 kΩ | 0.226 V | 39.1 μA |
| M5 | 3360 μΩ⁻¹ | 35.7 kΩ | 0.226 V | 411 μA |
| M6 | 4370 μΩ⁻¹ | 17.5 kΩ | 0.179 V | 411 μA |
| M7 | 95.1 μΩ⁻¹ | 980.4 kΩ | 0.185 V | 9.2 μA |

### Key Simulated Metrics

| Metric | Simulated | Hand-Calculated | Error |
|--------|-----------|-----------------|-------|
| DC Gain | 67.07 dB (2256.83) | 67.95 dB (2500) | −9.73% |
| Phase Margin | 64.12° | 70.25° | — |

---

## Passive Components

| Component | Value | Purpose |
|-----------|-------|---------|
| C (Miller cap) | 2 pF | Frequency compensation |
| Rz | 217.3 Ω | RHP zero cancellation |
| CL | 5 pF | Load capacitor |
| Rf | 500 kΩ | Feedback resistor (closed-loop) |
| R0 | 500 kΩ | Input bias resistor (closed-loop) |

---

## Power Consumption

| Parameter | Value |
|-----------|-------|
| Total current (Iref + I3 + I4 + I5) | 561.2 μA |
| Supply voltage VDD | 1.8 V |
| **Total power** | **1.010 mW** |

---

## Voltage Swing Limits

| Limit | Value |
|-------|-------|
| Input lower limit | Vin ≥ 0.77 V |
| Input upper limit | Vin ≤ 1.58 V |
| Output lower limit | Vout ≥ 0.2 V |
| Output upper limit | Vout ≤ 1.6 V |

---

## Closed-Loop Configuration (Gain = 2)

Non-inverting amplifier with Rf = R0 = 500 kΩ:

```
A_CL = 1 + Rf/R0 = 1 + 1 = 2
```

Input biased at VDD/2 = 0.9 V. Transient test: 0.2 V step input (PULSE from 0.9 V to 1.1 V, rise/fall = 0.1 ns).

**Observed transient:** Output settles from ~0.9 V to ~1.1 V (×2 gain verified), with clean settling and no significant overshoot.

---

## Files

| File | Description |
|------|-------------|
| `OTA_OPEN_LOOP.asc` | LTSpice schematic — open-loop AC analysis (Bode plot) |
| `OTA_CLOSED_LOOP.asc` | LTSpice schematic — closed-loop transient analysis (gain = 2) |
| `REPORT_OTA.pdf` | Full hand-written design report with calculations |

### Running the Simulations

1. Open the `.asc` files in **LTSpice XVII** (or later).
2. Ensure `tsmc018.lib` is in the same directory or on the LTSpice library path.
3. **Open-loop:** Run `.ac dec 100 1 200meg` → view V(n005) for Bode plot (magnitude + phase).
4. **Closed-loop:** Run `.tran 10u` → view V(n005) for transient step response.

---

## Design Notes

- All transistor lengths set to L = 0.5 μm (≥ 2·Lmin) to improve output resistance and reduce channel-length modulation effects.
- The simulated gain (~67 dB) is ~10% below the theoretical value; increasing L while maintaining W/L ratios would bring it closer to target.
- Phase margin of ~64° (simulated) satisfies the >60° stability requirement.
- The Rz resistor eliminates the right-half-plane zero introduced by the Miller capacitor.
