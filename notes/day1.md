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
<br><br><br><br>

# Explanations:

### 1. Core vs Processor (no ambiguity allowed)

**Processor (historical / system-level term)**
A *processor* is the entity that executes programs and is visible at the system boundary. Depending on context, it may contain:

* One or more **cores**
* Shared caches
* Memory controllers
* Interconnect logic
* I/O interfaces

In modern systems, “processor” ≈ **chip-level abstraction**.

**Core (microarchitectural term)**
A *core* is an independent instruction execution engine with:

* Its own **program counter**
* Its own **register file**
* Its own **pipeline**
* Its own control logic

A core can execute an independent instruction stream (thread / hart).

**RISC-V precision**
RISC-V avoids ambiguity and uses the term **hart** (Hardware Thread).

* 1 core may implement **1 or more harts**
* In FPGA softcores, usually **1 core = 1 hart**

For your project, unless you explicitly add SMT (you shouldn’t initially):

> **1 core = 1 hart = 1 independent instruction stream**

If you misuse these terms later, I will correct you.

---

### 2. Single-core vs Multicore (architectural consequences)

#### Single-core

* One hart
* One instruction stream
* No true parallel execution
* Simpler:

  * No cache coherence
  * No synchronization primitives required (beyond interrupts)

Performance scaling:

* Only via clock frequency
* Or instruction-level parallelism (pipeline, superscalar — unlikely on FPGA)

#### Multicore

* Multiple independent harts
* Parallel execution of software
* Requires **explicit architectural support**:

  * Memory consistency model
  * Synchronization (locks, atomics, barriers)
  * Interconnect arbitration

Performance scaling:

* Limited by:

  * Memory bandwidth
  * Synchronization overhead
  * Algorithm parallelizability (Amdahl’s Law)

For image reconstruction, multicore is **mandatory** if you want usable throughput.

---

### 3. Shared Memory Model (exact meaning)

A **shared memory model** means:

* All cores see **the same physical address space**
* A store by Core A can be loaded by Core B
* Communication is via **loads/stores**, not messages

This does **not** imply:

* Automatic cache coherence
* Strong ordering
* High performance

You must choose:

* **Coherent shared memory**
* **Non-coherent shared memory**

On FPGA soft RISC-V systems:

* **Non-coherent shared memory is the default**
* Coherence is expensive and often avoided

#### Minimal shared-memory system (typical FPGA)

* Each core:

  * Private instruction memory
  * Private data memory or cache
* Shared:

  * One external DRAM (or BRAM)
  * Simple interconnect (AXI, Wishbone, crossbar)

Synchronization is done using:

* Atomic instructions (RISC-V A extension), **or**
* Software protocols with memory-mapped locks

---

### 4. What you must draw (no excuses)

Your diagram must include **at minimum**:

#### Per-core blocks (replicated N times)

* RISC-V Core (label as “RV32/64 Core” or “Hart”)
* Register file (can be implicit inside core)
* Instruction interface
* Data interface

#### Shared blocks

* Interconnect (AXI crossbar / shared bus)
* Shared memory (BRAM or DDR controller)
* Optional:

  * UART
  * Timer
  * Interrupt controller (PLIC / CLINT)

#### Explicitly show

* Which memories are **private**
* Which memory is **shared**
* How cores connect to shared memory

If you omit interconnect arbitration or pretend multiple masters magically talk to memory, that is incorrect.

---

### 5. Reference architecture (textual, for translation to diagram)

```
           +-------------------+
           |   Shared Memory   |
           | (BRAM / DDR Ctrl) |
           +---------+---------+
                     |
              +------+------+
              | Interconnect |
              | (AXI/XBAR)  |
              +------+------+
        +------------+------------+
        |                         |
+-------+-------+         +-------+-------+
|   Core 0      |         |   Core 1      |
|  (RISC-V)     |         |  (RISC-V)     |
|               |         |               |
| I-Mem / Cache |         | I-Mem / Cache |
| D-Mem / Cache |         | D-Mem / Cache |
+---------------+         +---------------+
```

If you add more cores, the pattern repeats.

---

### 6. Deliverable standard (strict)

Your diagram must:

* Be **legible**
* Use **clear labels**
* Show **data paths**, not just boxes
* Be something you can explain signal-by-signal later

When you’re done, your next message should include:

* A photo or description of the diagram
* OR a precise textual breakdown of every block you drew

I will then:

* Critique it
* Identify architectural flaws
* Force corrections before Day 2

Proceed.
