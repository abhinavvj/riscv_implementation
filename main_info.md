# Note
These are ChatGPT rendered answers for a two month timeline.

There are 3 sections - roadmap, categorical, references.

In [roadmap](#roadmap), a day by day roadmap to proceed with the topic.

In [concepts](#concepts), ChatGPT assumes the role of a tutor and lists important topics and steps categorically, which can be used to refer progress.

In [references](#references), the materials are mapped categorically and has specific portions suggested to study to ensure maximum efficiency

In [softwares](#softwares), the necessary softwares to be installed for this project are given. Scroll to the end of the section to see the bare minimum ones

All the files in the [**notes**](#notes/) folder will have ChatGPT generated content, where it assumes the role of the tutor. 

Use WSL - Ubuntu 22.04 here to ensure compatibiltiy and interview preparedness

<br><br><br><br>

# Roadmap

Below is a **strict, day-by-day execution plan starting Feb 5**, optimized for **2 hours/day**, **first-time domain exposure**, and the updated title:

> **Hardware Implementation of Multicore RISC-V for Image Reconstruction**

This plan is intentionally **constrained**. Each day has:

* **What to study** (time-boxed)
* **What to do** (hands-on)
* **A hard deliverable** (non-negotiable)
* **A stop condition** (prevents overwork / scope creep)

If you follow this exactly, you will reach a **defensible hardware demo** with solid theory by the end of March.

---

# GLOBAL RULES (DO NOT IGNORE)

1. **2 hours max/day** — stop when time is up
2. **If a deliverable fails → repeat same day next slot**
3. **No optimization until correctness is proven**
4. **Do not jump ahead**

---

# PHASE 1 — FOUNDATION (Feb 5 → Feb 14)

Goal: *Understand what you are building before touching multicore.*

---

## **Day 1 — Feb 5**

**Study (45 min)**

* What is a core vs processor
* Single-core vs multicore
* Shared memory model

**Do (60 min)**

* Draw block diagram of a multicore RISC-V system on FPGA

**Deliverable**

* One clear diagram (paper or digital)

**Stop**

* Do NOT install or run anything else

---

## **Day 2 — Feb 6**

**Study (45 min)**

* Amdahl’s Law (formula + intuition)

**Do (60 min)**

* Write a paragraph:

  > Why adding cores does not give linear speedup

**Deliverable**

* Written explanation (own words)

**Stop**

* No coding

---

## **Day 3 — Feb 7**

**Study (60 min)**

* RISC-V RV32I basics
* Register file
* Load/store architecture

**Do (45 min)**

* Compile simple C → RISC-V assembly (Compiler Explorer)

**Deliverable**

* Annotated assembly snippet

---

## **Day 4 — Feb 8**

**Study (45 min)**

* RISC-V calling convention

**Do (60 min)**

* Identify stack frame in RISC-V assembly

**Deliverable**

* Diagram of function call flow

---

## **Day 5 — Feb 9**

**Study (60 min)**

* Race conditions
* Critical sections

**Do (45 min)**

* Hand-simulate two cores incrementing a variable

**Deliverable**

* Step-by-step failure explanation

---

## **Day 6 — Feb 10**

**Study (45 min)**

* Spinlocks and barriers

**Do (60 min)**

* Write C pseudocode for:

  * Spinlock
  * Barrier

**Deliverable**

* Correct pseudocode

---

## **Day 7 — Feb 11**

**Study (30 min)**

* FPGA vs ASIC
* BRAM vs DRAM

**Do (75 min)**

* Map where code, data, and image live in FPGA memory

**Deliverable**

* Memory map diagram

---

## **Day 8 — Feb 12**

**Study (45 min)**

* What “hardware implementation” actually means

**Do (60 min)**

* Write 5 viva-ready sentences defending the title

**Deliverable**

* Defense notes

---

## **Day 9 — Feb 13**

**Study (45 min)**

* VexRiscv architecture overview

**Do (60 min)**

* Clone VexRiscv repo
* Read folder structure (no build yet)

**Deliverable**

* Notes on modules

---

## **Day 10 — Feb 14**

**Study (30 min)**

* SoC boot flow

**Do (75 min)**

* Build & simulate **single-core** VexRiscv example (Verilator)

**Deliverable**

* “Hello World” in simulation

---

# PHASE 2 — HARDWARE BASELINE (Feb 15 → Feb 28)

Goal: *Single-core RISC-V running on FPGA.*

Wherever the original plan says:

“Install toolchain”
“Compile C”
“Run simulation”

Interpret it as:

Do it inside WSL Ubuntu
Using Linux commands
Through VS Code (WSL window)

Wherever it says: “Generate Verilog”
Interpret it as:

Generate in WSL
Copy/export to a Windows-visible folder

---

## **Day 11–12 — Feb 15–16**

**Do (2 hrs/day)**

* Integrate VexRiscv with Vivado
* Generate bitstream

**Deliverable**

* Bitstream generated successfully

**Limit**

* No multicore yet

---

## **Day 13 — Feb 17**

**Do**

* UART output on FPGA

**Deliverable**

* “Hello World” on board

---

## **Day 14–15 — Feb 18–19**

**Study (30 min/day)**

* Cycle counters

**Do (90 min/day)**

* Measure execution cycles

**Deliverable**

* Baseline cycle count

---

## **Day 16 — Feb 20**

**Do**

* Document single-core architecture

**Deliverable**

* Block diagram + explanation

---

## **Day 17–18 — Feb 21–22**

**Do**

* Enable **second core**
* Core ID-based execution

**Deliverable**

```
Core 0 alive
Core 1 alive
```

---

## **Day 19 — Feb 23**

**Study**

* Memory contention basics

**Do**

* Shared memory test (no locks)

**Deliverable**

* Demonstrated race condition

---

## **Day 20 — Feb 24**

**Do**

* Implement spinlock in hardware-running code

**Deliverable**

* Correct shared counter

---

## **Day 21 — Feb 25**

**Do**

* Barrier implementation

**Deliverable**

* Deterministic execution

---

## **Day 22 — Feb 26**

**Do**

* Document multicore design

**Deliverable**

* Updated block diagram

---

## **Day 23–24 — Feb 27–28**

**Do**

* Stabilization & backup

**Deliverable**

* Working multicore demo (recorded)

---

# PHASE 3 — IMAGE RECONSTRUCTION (Mar 1 → Mar 20)

Goal: *Parallel image reconstruction on FPGA.*

Wherever the plan says:

“Synthesize”
“Generate bitstream”
“Program FPGA”

Interpret it as:

Vivado on Windows
Import Verilog generated in WSL
Import .elf / .bin built in WSL

---

## **Day 25–26 — Mar 1–2**

**Study**

* Simple image reconstruction algorithm

**Do**

* Implement single-core version

**Deliverable**

* Correct output

---

## **Day 27–28 — Mar 3–4**

**Do**

* Parallelize reconstruction

**Deliverable**

* Correct parallel output

---

## **Day 29–30 — Mar 5–6**

**Do**

* Performance measurement

**Deliverable**

* Speedup data

---

## **Day 31–32 — Mar 7–8**

**Do**

* Resource utilization analysis

**Deliverable**

* FPGA reports

---

## **Day 33–34 — Mar 9–10**

**Do**

* Documentation draft

**Deliverable**

* 70% report complete

---

# PHASE 4 — EXAM BUFFER & FINALIZATION (Mar 11 → Mar 31)

Last week of March = exams → **low-risk only**

C code → compiled in WSL
Binary → copied to Windows
FPGA run → Vivado

---

## **Mar 11–20**

* Viva prep
* Final graphs
* Demo stabilization

---

## **Mar 21–31 (Exam-safe)**

* No hardware changes
* Only documentation and rehearsal

---

# FINAL NOTE (IMPORTANT)

If you fall behind:

* **Do NOT compress future days**
* Repeat missed days
* Cut features, not understanding

---
<br><br><br><br><br>


# Concepts

I will act as a technical tutor at an expert level for **hardware implementation of a multicore RISC-V system for image reconstruction**.

I will:

* Assume **no prior correctness** in what you state.
* Explicitly identify and **fill prerequisite gaps** (digital design, computer architecture, RISC-V ISA, memory systems, parallelism, image reconstruction math, hardware/software co-design).
* Be **precise, formal, and technical**.
* Correct errors immediately, with justification.
* State uncertainty explicitly if it exists.
* Double-check logic, math, and code before presenting it.
* Avoid hand-waving, motivational language, or affirmation without proof.

Scope we will likely cover (non-exhaustive, as required for this project):

1. **Image reconstruction fundamentals**

   * Linear systems, inverse problems
   * Convolution, FFT, back-projection, iterative solvers
   * Numerical precision and error propagation

2. **Algorithm → hardware mapping**

   * Dataflow analysis
   * Parallelism extraction (task, data, pipeline)
   * Memory access patterns and bandwidth constraints

3. **RISC-V architecture**

   * ISA subsets (RV32I/RV64I, M, A, F, D, V)
   * Privilege levels
   * Memory model
   * Custom instructions and accelerators

4. **Multicore design**

   * Cache coherence (MESI variants or absence thereof)
   * Interconnects (bus, crossbar, NoC)
   * Synchronization primitives
   * Determinism vs throughput tradeoffs

5. **Hardware implementation**

   * RTL (Verilog/SystemVerilog)
   * Timing closure and pipelining
   * Area, power, and frequency tradeoffs
   * FPGA vs ASIC constraints

6. **Toolchains**

   * RISC-V GCC/LLVM
   * Bare-metal vs RTOS
   * Simulation and verification

7. **Performance analysis**

   * Roofline modeling
   * Amdahl/Gustafson limits
   * Fixed-point vs floating-point implications

This will be demanding. If something is missing from your background, I will stop and teach it before proceeding.

---

# References

Below is a **curated, minimal, high-signal reference list** mapped exactly to the weekly plan and topics. These are **not random links**—each resource was chosen because it gives **maximum understanding per hour**, which is what you need given time pressure, exams, and expectations.

I am grouping by **topic**, not by week, so you can revisit them as needed.

---

# 1. Multicore & Computer Architecture Fundamentals

### Goal

Understand *why* multicore works, *where* it fails, and *how* memory dominates performance.

### Core Resources (use all 3)

1. **“Computer Architecture: A Quantitative Approach” – Hennessy & Patterson**

   * Chapters:

     * Multicore processors (overview)
     * Amdahl’s Law
   * Why: This is the **gold standard**. Interviewers implicitly expect its mental model.

2. **MIT 6.004 / 6.823 Lecture Notes (Multicore Intro)**

   * Why: Explains multicore with clarity, not math overload.

3. **Stanford CS149 – Parallel Computing (Intro lectures)**

   * Why: Excellent intuition for speedup limits and memory bottlenecks.

### Practice

* Write your own explanation:

  > “Why adding cores eventually stops helping”
* Draw memory access timelines for 2 cores

---

# 2. RISC-V ISA & Execution Model

### Goal

Be able to **trace C → assembly → execution** confidently.

### Core Resources

1. **RISC-V Reader – David Patterson & Andrew Waterman**

   * Focus on:

     * RV32I
     * Calling convention
   * Why: Concise, exam-friendly, authoritative.

2. **Official RISC-V ISA Manual (Volume I – Unprivileged)**

   * Only read:

     * Instruction formats
     * Load/store model
   * Why: Shows you can handle specs if asked.

3. **Compiler Explorer (godbolt.org)**

   * Compile C → RISC-V assembly
   * Why: Fastest way to understand execution.

### Practice

* Annotate one C function’s assembly:

  * Stack frame
  * Register saves
  * Return path

---

# 3. Memory Systems & Synchronization (CRITICAL)

### Goal

Understand race conditions, atomicity, and why multicore bugs are subtle.

### Core Resources

1. **“What Every Programmer Should Know About Memory” – Ulrich Drepper**

   * Sections on memory ordering
   * Why: Makes memory behavior *click*.

2. **MIT 6.824 – Concurrency Basics (selected lectures)**

   * Why: Clean explanation of races and locks.

3. **Intel/ARM Memory Ordering Articles (conceptual)**

   * You don’t need ISA specifics—only principles.

### Practice

* Manually simulate:

  * Two cores incrementing a variable
* Write pseudocode for:

  * Spinlock
  * Barrier

If you can *explain* this, you are ahead of most students.

---

# 4. Digital Design for Multicore Systems

### Goal

Integrate and reason about hardware you did not write.

### Core Resources

1. **FPGA4Student – Multicore & Bus Tutorials**

   * Why: Simple, visual explanations.

2. **ZipCPU Blog (by Dan Gisselquist)**

   * Topics:

     * Bus design
     * Multicore pitfalls
   * Why: Written by someone who builds real CPUs.

3. **VexRiscv Documentation**

   * Why: Directly relevant to your project.

### Practice

* Draw block diagram:

  * Cores
  * Shared memory
  * Interconnect

---

# 5. RISC-V Toolchain & Bare-Metal Development

### Goal

Comfortably build, run, and debug without an OS.

### Core Resources

1. **RISC-V Bare-Metal Programming Guides**

   * Startup code
   * Linker scripts

2. **VexRiscv Examples Repository**

   * Do not modify initially
   * Why: Proven working reference.

3. **GNU RISC-V Toolchain Documentation**

   * Only basics: gcc, objdump, objcopy

### Practice

* Modify linker script slightly
* Print memory addresses in C

---

# 6. Multicore Bring-Up & Debugging

### Goal

Make multiple cores run *reliably*.

### Core Resources

1. **VexRiscv SMP Examples**

   * Why: Shows how cores are instantiated and booted.

2. **Low-Level Debugging Blogs (bare-metal multicore)**

   * Why: Teaches systematic debugging thinking.

3. **Open-Source Multicore RISC-V Projects**

   * Skim, don’t deep dive.

### Practice

* Core ID–based execution paths
* Shared flag synchronization

---

# 7. Parallel Programming Concepts

### Goal

Know how to split work correctly and safely.

### Core Resources

1. **CS149 Parallel Programming Lectures**

   * Why: Practical mental models.

2. **OpenMP Tutorials (conceptual only)**

   * Ignore syntax
   * Focus on *ideas*.

3. **“Structured Parallel Programming” (selected sections)**

### Practice

* Manually partition:

  * Image
  * Matrix
* Identify synchronization points

---

# 8. Object Reconstruction (Keep It Controlled)

### Goal

Understand algorithm enough to parallelize it.

### Core Resources

1. **Introductory Computer Vision Notes (Reconstruction / Back-projection)**

   * Why: Math-light, concept-heavy.

2. **Numerical Linear Algebra Notes**

   * Matrix accumulation
   * Why: Reconstruction is often just math + memory.

3. **Simple Image Processing Tutorials**

   * Avoid deep learning.

### Practice

* Implement single-core reconstruction
* Identify parallelizable loops

---

# 9. Performance Evaluation & Analysis

### Goal

Defend results logically, not emotionally.

### Core Resources

1. **Hennessy & Patterson – Performance Chapter**
2. **Amdahl’s Law Explained (multiple sources)**
3. **Basic Profiling Techniques (cycle counters)**

### Practice

* Plot speedup vs cores
* Explain deviation from ideal scaling

---

# HOW TO USE THIS LIST (IMPORTANT)

Do **not** read everything end-to-end.

For each week:

1. Pick **2 resources**
2. Skim for concepts
3. Immediately apply to your project
4. Write notes in your own words

If you understand it well enough to **teach it**, you understand it enough for viva and interviews.

---

# Softwares

Below is the **clean, final, authoritative list of software** you need for this project, **filtered for Windows + WSL2**, and **nothing extra**. Every item listed has a clear purpose in *your exact project*.

---

## 1. Host Operating System (Windows)

### 1.1 Windows 10 / 11

**Role:** Primary OS
**Why needed:**

* Runs Vivado (FPGA synthesis & programming)
* Stable USB/JTAG support for FPGA boards

---

## 2. Linux Environment (MANDATORY)

### 2.1 WSL2 (Windows Subsystem for Linux)

**Role:** Linux runtime inside Windows
**Why needed:**

* RISC-V toolchains are Linux-first
* Clean build environment
* Industry-standard workflow

---

### 2.2 Ubuntu 22.04 (inside WSL2)

**Role:** Development OS
**Why needed:**

* Best compatibility with RISC-V tools
* Well-documented and stable

---

## 3. FPGA Toolchain (Hardware Implementation)

### 3.1 Xilinx Vivado

**Role:** FPGA synthesis, implementation, bitstream generation
**Why needed:**

* Converts Verilog to hardware
* Programs FPGA board
* Generates timing and resource reports

> Runs **only on Windows**

---

## 4. RISC-V Development Toolchain (inside WSL)

### 4.1 RISC-V GNU Toolchain (`riscv32-unknown-elf-*`)

**Role:** Cross-compilation
**Why needed:**

* Compiles bare-metal C to RISC-V binaries
* Produces `.elf` / `.bin` for FPGA execution

---

### 4.2 GNU Binutils (comes with toolchain)

Includes:

* `objdump`
* `objcopy`
* `ld`

**Why needed:**

* Inspect assembly
* Control memory layout
* Debug startup code

---

## 5. RTL Simulation & Debugging (inside WSL)

### 5.1 Verilator

**Role:** Cycle-accurate RTL simulation
**Why needed:**

* Validate RISC-V system before FPGA
* Debug multicore behavior early

---

### 5.2 GTKWave

**Role:** Waveform viewer
**Why needed:**

* Visualize signals
* Understand memory access, synchronization, core activity

---

## 6. RISC-V Core / SoC Framework

### 6.1 VexRiscv

**Role:** RISC-V core and multicore-capable SoC
**Why needed:**

* Proven open-source RISC-V implementation
* Multicore support
* FPGA-friendly

> Used as **IP**, not modified deeply

---

### 6.2 SpinalHDL (Indirect dependency)

**Role:** HDL generator
**Why needed:**

* VexRiscv is written in SpinalHDL
* Generates synthesizable Verilog

> You do **not** need deep SpinalHDL knowledge

---

## 7. Source Control & Build Utilities (inside WSL)

### 7.1 Git

**Role:** Version control
**Why needed:**

* Manage large open-source repositories
* Track changes and recover from mistakes

---

### 7.2 GNU Build Tools

Includes:

* `make`
* `gcc`
* `ld`

**Why needed:**

* Build automation
* Reproducible compilation

---

## 8. Code Editor & IDE

### 8.1 Visual Studio Code

**Role:** Code editor
**Why needed:**

* Edit C, linker scripts, HDL
* Integrates with WSL

---

### 8.2 VS Code – WSL Extension

**Role:** Bridge between Windows and Linux
**Why needed:**

* Edit Linux files directly
* Single-editor workflow

---

## 9. Debugging (Optional but Strongly Recommended)

### 9.1 GDB / gdb-multiarch (inside WSL)

**Role:** Debugger
**Why needed:**

* Inspect registers
* Step through RISC-V execution
* Excellent interview talking point

---

## 10. Data Analysis & Visualization

### 10.1 Python 3

**Role:** Performance analysis
**Why needed:**

* Plot speedup graphs
* Analyze execution cycles

---

### 10.2 Python Libraries

* `matplotlib`
* `numpy`

**Why needed:**

* Clean performance plots
* Quantitative analysis

---

## 11. Documentation & Reporting

### 11.1 Markdown / LaTeX / Word

**Role:** Documentation
**Why needed:**

* Project report
* Architecture explanation
* Viva preparation

---

## 12. Optional (Understanding Aid)

### 12.1 Compiler Explorer (godbolt.org)

**Role:** Learning tool
**Why needed:**

* Understand C → RISC-V assembly mapping

---

# FINAL SUMMARY (MINIMAL SET)

If you want the **absolute minimum**:

### MUST-HAVE

* Windows
* WSL2 + Ubuntu 22.04
* Vivado
* VS Code + WSL extension
* RISC-V GNU Toolchain
* VexRiscv
* Verilator
* GTKWave
* Git

Everything else is support.
