# synchronous-fsm-digital-logic
IC-7474 flip-flop sequential finite state machine &amp; Simulink timing verification
4-Bit Synchronous Finite State Machine (74-Series TTL & Simulink)

A 4-bit synchronous finite state machine (FSM) designed, simulated at the gate level in MATLAB/Simulink, and built from scratch using discrete 74-series TTL logic ICs across two breadboards.

https://github.com/UMIengineer/synchronous-fsm-digital-logic/releases/tag/Video

Overview

The circuit implements a 12-state cyclic sequence with a single active output state (S3 = 0011) and self-starting recovery from unused states (S8, S9, S12). Rather than relying on a microcontroller or FPGA, all next-state combinational logic is built using multi-input NAND gates driving positive-edge-triggered D flip-flops.

State Sequence & Fault-Recovery Verification

Reset Initialization: Pressing the hardware reset button forces the flip-flops into S0 (0000).
Main Loop (12 States):
S0 (0000) ─▶ S10 (1010) ─▶ S13 (1101) ─▶ S15 (1111) ─▶ S4 (0100) ─▶ S2 (0010) ─▶ S14 (1110)
                ▲                                                                    │
                │                                                                    ▼
             S1 (0001) ◀─ S7 (0111) ◀─ S6 (0110) ◀─ S11 (1011) ◀─ S5 (0101) ◀─ S3 (0011) [Active = 1]
Unused-State Recovery Test (S8): To verify the FSM cannot get trapped in an unmapped state due to power-on glitches or noise, +5 V was injected directly onto the QD output to force the circuit into unused state S8 (1000). On subsequent clock pulses, the circuit automatically recovered back into the main loop (S8 → S11 → S6 → S7).
Hardware Build & Breadboard Architecture

The physical circuit is split across two breadboards (Board B6) powered by a +5 V rail:

Left Breadboard — Combinational Next-State Logic

IC	Function	Qty	Board Labels
7400	Quad 2-Input NAND Gate	3	7400-1, 7400-2, 7400-3
7410	Triple 3-Input NAND Gate	2	7410-1, 7410-2
7420	Dual 4-Input NAND Gate	1	7420-1
7430	8-Input NAND Gate	1	7430-1
Right Breadboard — State Memory, Clock & Controls

Component	Function	Details
3× 7474	Dual D-Type Flip-Flops	Labeled 7474-1, 7474-2, 7474-3; stores the 4 state bits (QD, QC, QB, QA)
Clock Source	System Clock + Indicator	Drives all flip-flop clock inputs synchronously; monitored via yellow pulse LED
Push-Buttons	Tactile SET & RESET	Replaced manual jumper-wire resets with dedicated tactile switches for reliable state testing
Status LEDs	Binary & Output Readout	4× Red LEDs (QD–QA current state) + 1× Green LED (active output at S3 = 0011)
Bus Color-Coding

To keep debugging manageable across ~100 jumper connections, every flip-flop output and complement line was color-coded consistently between boards:

QD / Q̄D: Grey / White
QC / Q̄C: Brown / Purple
QB / Q̄B: Yellow / Blue
QA / Q̄A: Orange / Pink
Power & Interconnects: Red (+5 V), Black (GND), Green (inter-gate & clock routing)
Simulink Verification & Pin-Mapping Convention

Before wiring the breadboards, the gate-level schematic was built and verified in MATLAB/Simulink using a custom netlist-style naming format on every gate:
<img width="507" height="636" alt="DE Circuit" src="https://github.com/user-attachments/assets/3959ea7e-4478-4f8b-a8df-8aa88d2ef19a" />
