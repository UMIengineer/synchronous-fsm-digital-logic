# synchronous-fsm-digital-logic
IC-7474 flip-flop sequential finite state machine &amp; Simulink timing verification
4-Bit Synchronous Finite State Machine (74-Series TTL & Simulink)

A 4-bit synchronous finite state machine (FSM) designed, simulated at the gate level in MATLAB/Simulink, and built from scratch using discrete 74-series TTL logic ICs across two breadboards.

(Paste your compressed .mp4 link on the line below after dragging the video into a GitHub issue/comment box to render the inline video player)

https://github.com/user-attachments/assets/your-video-id-here

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

<IC Part Number>-<Physical Chip #>-<Internal Gate #>
For example, 7400-1-1 maps directly to the 7400 quad 2-input NAND, 1st chip from the top on the left breadboard, gate #1 (pins 1, 2, 3). Combined with hand-drawn pinout sheets for the 7400, 7404, 7410, 7420, 7430, and 7474, any logic node in the Simulink simulation could be probed directly on the physical IC pins during fault-finding.

Circuit Diagrams & Pinouts

(Optional: save screenshots from 01:28, 01:46, and 01:50 of the demo video into a docs/ folder to display them below)

docs/simulink_schematic.png — Gate-level Simulink logic diagram with chip/gate labels
docs/breadboard_ic_map.png — Annotated physical IC placement across both breadboards
docs/ic_pinout_reference.png — Gate-to-pin mapping reference sheet for 74-series ICs
