# Simulated-Robotic-Pick-and-Place-RSLogix-500-
This ladder logic project simulates a simple robotic pick-and-place cycle using three sequenced actions: **arm extension**, **gripper closure**, and **arm retraction**. Each action is controlled by a dedicated timer, and the full sequence completes in approximately 7 seconds. Introduces core sequencing logic used in real industrial robot cells.

---

## 🧠 Core Concepts Demonstrated

- `TON`-based step sequencing (Extend → Gripper → Retract)
- One-shot `ONS` logic to initiate the cycle
- Output interlocking via memory bits
- Use of `.DN` bits to cascade motion phases
- Clean state-based system design

---

## 🧾 System Description

### 🟦 Inputs
| Tag       | Description           |
|-----------|-----------------------|
| `B3:0/0`  | Start Button (Momentary) |

### 🟥 Outputs
| Tag       | Description                |
|-----------|----------------------------|
| `B3:0/3`  | Extend Motor Output        |
| `B3:0/5`  | Gripper Close Output       |
| `B3:0/4`  | Retract Motor Output       |
| `B3:0/6`  | Done Bit (Sequence Complete) |

---

## 🔧 Ladder Logic Breakdown

### 🔁 Rung 0 – Start Button (One-Shot Trigger) ladder
[ B3:0/0 ] ----[ONS B3:0/2]---- ( B3:0/1 )
Momentary press of Start button sets Start Trigger (B3:0/1)

### 🔁 Rung 1 – Extend Motor ON for 3 Seconds ladder

BST [ B3:0/1 ]
NXB [ B3:0/3 ]
XIO T4:0/DN
BND
[ TON T4:0 Preset: 3s ]
( B3:0/3 )  ; Extend Motor ON
Starts Extend timer when sequence begins

Motor ON for 3 seconds, then proceeds

### 🔁 Rung 2 – Gripper Close ON for 1 Second ladder

BST [ T4:0/DN ]
NXB [ B3:0/5 ]
XIO T4:1/DN
BND
[ TON T4:1 Preset: 1s ]
( B3:0/5 )  ; Gripper Close ON
After extend is done, activates Gripper for 1 second

### 🔁 Rung 3 – Retract Motor ON for 3 Seconds ladder

BST [ T4:1/DN ]
NXB [ B3:0/4 ]
XIO T4:2/DN
BND
[ TON T4:2 Preset: 3s ]
( B3:0/4 )  ; Retract Motor ON
After gripper, begins retraction for 3 seconds

### 🔁 Rung 4 – Sequence Complete Indicator ladder

[ T4:2/DN ] ---- ( B3:0/6 )
When full motion completes, set DONE bit to indicate process finished

### 💡 Real-World Applications
Robotic pick-and-place simulation

Pneumatic cylinder sequencing

Motion coordination in packaging, assembly, or sorting lines

### 🛠 Tools Used
RSLogix Micro Starter Lite

RSLinx Classic

RSLogix Emulate 500


