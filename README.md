# Lab 7-B: Embedded C Programming on AT89C5131 Microcontroller

**Student:** Chinmay Purohit  
**Roll Number:** 23B1285  


---

## 📌 Overview

This lab explores the design and implementation of an embedded C program on the **AT89C5131 microcontroller**. The primary goals include:

- Interfacing a **4x4 keypad matrix**
- Displaying input characters on an **LCD**
- Generating output signals at frequencies associated with the keypad buttons using **Timer1**

This system simulates a **musical keypad** where each button corresponds to a distinct note (frequency).

---

## 🧾 Table of Contents

1. [Included Libraries and Definitions](#included-libraries-and-definitions)
2. [Keypad Matrix and Frequency Table](#keypad-matrix-and-frequency-table)
3. [Keypad Scanning and Display](#keypad-scanning-and-display)
4. [Timer1 and Frequency Generation](#timer1-and-frequency-generation)
   - [Cycle Calculation](#cycle-calculation)
5. [Main Function Workflow](#main-function-workflow)
6. [Conclusion](#conclusion)

---

## 📚 Included Libraries and Definitions

The code begins by including the following headers:

```c
#include <at89c5131.h>   // Microcontroller-specific functions and registers
#include <stdio.h>       // Standard I/O functions
#include "lcd.h"         // Custom LCD control library
```

### Key Variables

- **`keyboard[4][4]`** — Character mapping of keypad buttons  
- **`Freq[4][4]`** — Frequency mapping for corresponding keypad positions

These variables facilitate detecting user inputs and determining the output frequency.

---

## 🎹 Keypad Matrix and Frequency Table

The keypad is structured as a 4x4 matrix:

```c
char keyboard[4][4] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

int Freq[4][4] = {
  {262, 294, 330, 349},
  {392, 440, 494, 523},
  {587, 659, 698, 784},
  {880, 988, 1047, 1175}
};
```

Each button press results in a frequency being selected to simulate a musical tone.

---

## 🔍 Keypad Scanning and Display

Key detection is handled by the `find_pos()` function. It works by:

1. Grounding one **row** at a time via **Port 3 (P3)**
2. Reading the **column** states to identify the pressed key

When a key is pressed:
- Its character is displayed on the LCD using:
  - `lcd_cmd()`
  - `lcd_write_string()`

---

## ⏱️ Timer1 and Frequency Generation

The **Timer1 ISR** (`timer1_isr`) is used to toggle output pin **P0.7** at a frequency selected from the keypad. 

Timer1 is configured using:

```c
TMOD = 0x10;          // Timer1 in Mode 1 (16-bit)
TH1 = (cycles >> 8);  // High byte
TL1 = (cycles & 0xFF); // Low byte
```

### 🔢 Cycle Calculation

To generate a given frequency:

```c
num_cycles = -1000000 / Freq[row][col];
```

This result is split into high/low bytes and loaded into `TH1` and `TL1`.

---

## 🧠 Main Function Workflow

The `main()` function performs the following steps:

1. Initializes Timer1 in **Mode 1**
2. Enables global and Timer1 interrupts via `EA` and `ET1`
3. Waits for keypad input using `keyfind()`
4. Sets the selected frequency and starts toggling output via Timer1 ISR

---

## ✅ Conclusion

This project demonstrates a practical embedded system that:

- Reads user input from a **keypad**
- Displays information on an **LCD**
- Generates frequency-based outputs using a **timer interrupt**

It exemplifies integrating **I/O peripherals** with timer-controlled operations on the AT89C5131 microcontroller.

---

## 📷 Reference

> ![Keypad showing “Re”](insert-image-path-if-applicable)

---

## 🛠️ Requirements

- **AT89C5131 Microcontroller**
- 4x4 **Keypad**
- **16x2 LCD**
- Keil μVision IDE (for development and simulation)
- Flash Magic (for programming the MCU)

---

## 📁 Files

| File Name         | Description                            |
|------------------|----------------------------------------|
| `main.c`         | Embedded C code for keypad and timer   |
| `lcd.h`          | LCD control header                     |
| `lcd.c`          | LCD functions implementation           |
| `README.md`      | Documentation (this file)              |
| `LAB7_B_MICRO.pdf` | Lab report detailing system behavior  |

---

## ✍️ Author

**Chinmay Purohit**  
Roll No: 23B1285  
Dept. of Electrical Engineering  
IIT Bombay

---
