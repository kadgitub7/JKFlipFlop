# Introduction to JK Flip Flops

## Overview
The **JK Flip Flop** is an improvement over the SR Flip Flop. Its key advantage is that it eliminates the invalid state that occurs when both S and R inputs are set to `1`, replacing it with a useful **toggle** behaviour.

---

## Circuit Description
The JK Flip Flop is wired with feedback between its outputs and inputs:
- **Q'** is fed back as an input to the NAND gate responsible for producing **Q**
- **Q** is fed back as an input to the NAND gate responsible for producing **Q'**

For all input combinations **other than J = 1, K = 1**, the outputs behave identically to a standard SR Flip Flop.

---

## The J = 1, K = 1 Case (Toggle)
When **J = 1, K = 1, Clk = 1**, the feedback connections cause the outputs to **toggle** on each clock cycle rather than entering an invalid state.

**Example walkthrough:**

Assume the initial state is **Q = 0, Q' = 1** before the input is applied:
1. The first NAND gate receives `1` and `1` (J and Q') → produces `0`
2. The second NAND gate receives `1` and `0` (K and Q) → produces `1`
3. Therefore: **Q = 1, Q' = 0**

On the next clock cycle, the outputs will switch back to **Q = 0, Q' = 1**, and this toggling continues with each subsequent clock pulse.

> **Key takeaway:** The previously invalid state `S = 1, R = 1` is now defined as **Qn+1 = (Qn)'** — the complement of the current state.

---

## Race Around Condition

The toggling behaviour of the JK Flip Flop introduces a timing hazard known as the **Race Around Condition**. While toggling is controlled, the race around condition is **not** — the circuit can flip uncontrollably between values during a single clock pulse if the propagation delay is shorter than the clock's high period.

### Conditions to Avoid Racing

1. **Half time period constraint** — The half time period of the clock must be less than the propagation delay of the flip flop, preventing the output from feeding back and toggling multiple times in one pulse.
2. **Edge triggering** — By triggering only on the rising or falling edge of the clock, there is not enough time for the circuit to race, since the active window is extremely narrow.
3. **Master-Slave Flip Flops** — Using a master-slave configuration separates the capture and output stages, ensuring the feedback path is broken during the active clock phase.
![RacingDiagram](/RacingDiagram.png)
---

## Truth Table

| Clk | J | K | Qn+1          |
|-----|---|---|---------------|
| 0   | X | X | Qn (Memory)   |
| 1   | 0 | 0 | Qn (Memory)   |
| 1   | 0 | 1 | 0 (Reset)     |
| 1   | 1 | 0 | 1 (Set)       |
| 1   | 1 | 1 | (Qn)' (Toggle)|

> `X` denotes a *don't care* condition — the input value has no effect on the output.

This behaviour of the input **J = 1** and **K = 1** switching the saved values to the complements is called **toggling**.

---

## Characteristic Table
The characteristic table shows the next state **Qn+1** based on the current state **Qn** and inputs **J** and **K**:

| Qn | J | K | Qn+1 |
|----|---|---|------|
| 0  | 0 | 0 | 0    |
| 0  | 0 | 1 | 0    |
| 0  | 1 | 0 | 1    |
| 0  | 1 | 1 | 1    |
| 1  | 0 | 0 | 1    |
| 1  | 0 | 1 | 0    |
| 1  | 1 | 0 | 1    |
| 1  | 1 | 1 | 0    |

---

## Excitation Table
The excitation table shows the required inputs **J** and **K** to achieve a desired state transition from **Qn** to **Qn+1**:

| Qn | Qn+1 | J | K |
|----|------|---|---|
| 0  | 0    | 0 | X |
| 0  | 1    | 1 | X |
| 1  | 0    | X | 1 |
| 1  | 1    | X | 0 |

> `X` denotes a *don't care* condition.

---

## Karnaugh Maps

### J K-Map
Derived from the characteristic table, plotting **J** against **Qn / Qn+1**:

| Qn \ Qn+1 | 0 | 1 |
|-----------|---|---|
| 0         | 0 | 1 |
| 1         | X | X |

**1 group of 2:**

$$J = Q_{n+1}$$

### K K-Map
Derived from the characteristic table, plotting **K** against **Qn / Qn+1**:

| Qn \ Qn+1 | 0 | 1 |
|-----------|---|---|
| 0         | X | X |
| 1         | 1 | 0 |

**1 group of 2:**

$$K = (Q_{n+1})'$$

### Qn+1 K-Map
Plotting **Qn+1** against **Qn** and **JK** input combinations:

| Qn \ JK | 00 | 01 | 11 | 10 |
|---------|----|----|----|----|
| 0       | 0  | 0  | 1  | 1  |
| 1       | 1  | 0  | 0  | 1  |

---

## Characteristic Equation
From the Karnaugh map, the next state equation for the JK Flip Flop is:

$$Q_{n+1} = J \cdot (Q_n)' + Q_n \cdot K'$$

---

## Summary of Operating Modes

| J | K | Mode   | Description                        |
|---|---|--------|------------------------------------|
| 0 | 0 | Memory | Output retains its previous state  |
| 0 | 1 | Reset  | Output is forced to 0              |
| 1 | 0 | Set    | Output is forced to 1              |
| 1 | 1 | Toggle | Output switches to its complement  |
