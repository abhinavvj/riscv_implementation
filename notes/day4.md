## Day 4 — Feb 8

Study (45 min)

RISC-V calling convention

Do (60 min)

Identify stack frame in RISC-V assembly

Deliverable

Diagram of function call flow

---
<br><br><br><br>

# Explanation

## 1. RISC-V calling convention (RV32 ABI essentials)

### Register roles (non-negotiable)

**Caller-saved (volatile)**
Caller must assume these are clobbered by a call:

* `ra` (x1)
* `t0–t6` (x5–x7, x28–x31)
* `a0–a7` (x10–x17)

**Callee-saved (non-volatile)**
Callee must restore these before returning:

* `s0–s11` (x8–x9, x18–x27)
* `sp` (x2) must be preserved

If a function modifies a callee-saved register and does not restore it, the program is **incorrect**.

---

## 2. Stack basics (precise rules)

* Stack grows **downward** (toward lower addresses)
* `sp` always points to the **top of the current stack frame**
* Stack must be **16-byte aligned at call boundaries** (ABI requirement)

Violating alignment causes silent failure with some toolchains.

---

## 3. When a stack frame exists

A function **needs a stack frame** if it:

* Calls another function
* Uses more registers than available caller-saved ones
* Needs local variables that don’t fit in registers
* Needs to save `ra`

Leaf functions with minimal register use **may have no stack frame**.

---

## 4. Canonical stack frame layout (RV32)

High address

```
| caller frame            |
|-------------------------|
| saved ra                |  <-- sp + frame_size - 4
| saved s0 / fp           |
| local variables         |
| temporaries / spills    |
|-------------------------|
| <- sp (after alloc)     |
```

Low address

`fp` (`s0`) is optional but commonly used at `-O0`.

---

## 5. Example: function with a stack frame

C code:

```c
int f(int x) {
    int y = x + 1;
    return g(y);
}
```

### Prologue (stack frame creation)

```asm
f:
    addi sp, sp, -16      # allocate 16-byte stack frame
    sw   ra, 12(sp)      # save return address
    sw   s0, 8(sp)       # save frame pointer
    addi s0, sp, 16      # establish frame pointer
```

### Function body

```asm
    addi a0, a0, 1       # y = x + 1
    call g               # ra overwritten here
```

### Epilogue (stack frame destruction)

```asm
    lw   ra, 12(sp)      # restore return address
    lw   s0, 8(sp)       # restore frame pointer
    addi sp, sp, 16      # deallocate stack frame
    ret
```

---

## 6. Stack frame identification checklist (this is what you must apply)

When looking at **any** RISC-V function:

1. Find `addi sp, sp, -N`
   → stack frame allocation
2. Look for `sw ra, offset(sp)`
   → non-leaf function
3. Look for `sw sX, offset(sp)`
   → callee-saved registers in use
4. Match `lw` restores before `ret`
5. Confirm stack pointer returns to original value

If any of these are missing when they should be present, the code is broken.

---

## 7. Function call flow diagram (what your deliverable must show)

Your diagram must show **time-ordered flow**, not just boxes.

### Required stages

```
Caller
  |
  |-- place args in a0–a7
  |-- call f (ra ← PC+4)
  v
Callee: f
  |
  |-- Prologue
  |     sp ← sp - N
  |     save ra, s-registers
  |
  |-- Function body
  |     (may call others)
  |
  |-- Epilogue
  |     restore ra, s-registers
  |     sp ← sp + N
  |-- ret (PC ← ra)
  v
Caller resumes
```

### Explicitly label

* Where `ra` is written
* Where `sp` changes
* Which registers survive the call

---

## 8. Why this matters for multicore + FPGA

* Stack lives in **shared memory or private memory** — you must choose
* Interrupts and context switches **depend** on correct stack discipline
* Bugs here become **nondeterministic** under parallel execution

If you cannot trace a stack frame precisely, you are not ready for:

* Multicore scheduling
* Interrupt handlers
* Image reconstruction kernels with deep call stacks