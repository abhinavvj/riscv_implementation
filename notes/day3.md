## **Day 3 — Feb 7**

**Study (60 min)**

* RISC-V RV32I basics
* Register file
* Load/store architecture

**Do (45 min)**

* Compile simple C → RISC-V assembly (Compiler Explorer)

**Deliverable**

* Annotated assembly snippet

----
<br><br><br><br>

# Explanation

Below is the **minimum correct mental model** you must have before touching multicore or image reconstruction. Anything less will cause architectural mistakes later.

---

## 1. RV32I basics (only what matters)

**RV32I**:

* RISC-V
* 32-bit address space
* Integer-only base ISA
* Load/store architecture
* Fixed 32-bit instructions

No floating point. No SIMD. No magic.

---

## 2. Register file (exact facts)

RV32I has **32 architectural integer registers**, each **32 bits wide**.

| Name    | ABI    | Purpose                           |
| ------- | ------ | --------------------------------- |
| x0      | zero   | Hardwired to 0 (writes discarded) |
| x1      | ra     | Return address                    |
| x2      | sp     | Stack pointer                     |
| x3      | gp     | Global pointer                    |
| x4      | tp     | Thread pointer                    |
| x5–x7   | t0–t2  | Temporaries                       |
| x8      | s0/fp  | Saved / frame pointer             |
| x9      | s1     | Saved                             |
| x10–x17 | a0–a7  | Function arguments / return       |
| x18–x27 | s2–s11 | Saved                             |
| x28–x31 | t3–t6  | Temporaries                       |

**Key properties**:

* Register file is **single-cycle** in simple cores
* Reads are combinational, writes occur on clock edge
* x0 is physically implemented but tied to zero

If you don’t know *why* x0 exists, say so later and I will explain.

---

## 3. Load/store architecture (no ambiguity)

RV32I **cannot** operate directly on memory.

This is illegal:

```
add x5, 0(x10), 0(x11)   // DOES NOT EXIST
```

Correct flow:

1. Load memory → register
2. Operate on registers
3. Store register → memory

This rule is absolute.

---

## 4. Canonical example in C

Assume this C code (deliberately simple):

```c
int add(int *a, int *b) {
    return *a + *b;
}
```

You compiled something equivalent in Compiler Explorer targeting **rv32i**, no optimizations (`-O0` or `-O1`).

---

## 5. Annotated RV32I assembly (what your deliverable should look like)

```asm
add:
    lw   a0, 0(a0)     # load word from memory at address in a0
                       # a0 originally held pointer 'a'
                       # now a0 = *a

    lw   a1, 0(a1)     # load word from memory at address in a1
                       # a1 originally held pointer 'b'
                       # now a1 = *b

    add  a0, a0, a1   # register-register ALU operation
                       # a0 = *a + *b

    ret               # return to caller
                       # a0 holds return value by ABI
```

### Why this matters (hardware view)

* `lw` → memory access → interconnect → shared memory
* `add` → pure ALU, no memory
* `ret` → control-flow change (PC update)

In multicore:

* Those `lw` instructions are **where contention happens**
* The `add` instruction is effectively free by comparison

---

## 6. What I will check in *your* submission

Your annotated snippet must:

* Correctly identify **which registers hold pointers vs values**
* Explicitly note **memory vs register operations**
* Use **ABI names**, not only xN
* Not claim that arithmetic touches memory (common beginner error)

If your assembly contains:

* Pseudo-instructions you cannot explain
* Stack usage you don’t understand
* Optimizations you didn’t intend

I will stop and correct you before moving on.

---

## 7. Next step warning

From Day 4 onward:

* We will analyze **memory traffic per instruction**
* Then map that to **shared memory bottlenecks**
* Then tie it back to **Amdahl’s Law numerically**
