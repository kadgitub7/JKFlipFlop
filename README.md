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

## Truth Table

| Clk | J | K | Qn+1          |
|-----|---|---|---------------|
| 0   | X | X | Qn (Memory)   |
| 1   | 0 | 0 | Qn (Memory)   |
| 1   | 0 | 1 | 0 (Reset)     |
| 1   | 1 | 0 | 1 (Set)       |
| 1   | 1 | 1 | (Qn)' (Toggle)|

> `X` denotes a *don't care* condition — the input value has no effect on the output.

---

## Summary of Operating Modes

| J | K | Mode   | Description                        |
|---|---|--------|------------------------------------|
| 0 | 0 | Memory | Output retains its previous state  |
| 0 | 1 | Reset  | Output is forced to 0              |
| 1 | 0 | Set    | Output is forced to 1              |
| 1 | 1 | Toggle | Output switches to its complement  |