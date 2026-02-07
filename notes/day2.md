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
<br><br><br><br>

# Explanations

### Amdahl’s Law — exact statement

Let:

* ( P ) = fraction of execution time that is **perfectly parallelizable**
* ( 1 - P ) = fraction that is **strictly serial**
* ( N ) = number of cores

The **maximum theoretical speedup** is:

$$
S(N) = \frac{1}{(1 - P) + \frac{P}{N}}
$$

This is not a performance estimate. It is a **hard upper bound** assuming:

* Zero synchronization overhead
* Zero communication cost
* Infinite memory bandwidth
* Perfect load balance

Any real system performs **worse**.

$$
\text{As }  ( N \to \infty ):
$$

$$
\lim_{N \to \infty} S(N) = \frac{1}{1 - P}
$$

This is the most important result. If 10% of your workload is serial, your speedup is **capped at 10×**, regardless of core count.

---

### Intuition (non-handwavy)

Execution time is composed of **phases**, not instructions:

* Serial phases: initialization, reductions, synchronization, I/O
* Parallel phases: independent pixel computations, independent rays, independent iterations

Parallel hardware **cannot** overlap serial phases. They are on the critical path.

Adding cores only shrinks the parallel portion. The serial portion is untouched and increasingly dominates total time.

At high core counts, you are optimizing the wrong term.

---

### Why adding cores does not give linear speedup (model answer)

Adding cores does not result in linear speedup because only a fraction of any real workload can be executed in parallel, while the remaining fraction is inherently serial. According to Amdahl’s Law, the serial portion of execution time places a strict upper bound on achievable speedup, regardless of how many cores are added. As more cores are introduced, the parallel portion of the workload is divided among them and decreases proportionally, but the serial portion remains constant and eventually dominates total execution time. In addition, real multicore systems incur overheads such as synchronization, contention for shared memory bandwidth, and load imbalance, which further reduce scaling efficiency below the theoretical limit. As a result, performance improvement shows diminishing returns with increasing core count, making linear speedup physically unattainable beyond small numbers of cores.

---

### Why this matters for your RISC-V image reconstruction project

If you cannot explicitly identify:

* The serial fraction of your reconstruction algorithm
* Where synchronization occurs
* Where memory bandwidth becomes the bottleneck

Then multicore hardware design effort will be wasted silicon.

On Day 3, we will **quantify** ( P ) for an image reconstruction kernel and relate it to memory traffic on FPGA.

If your written paragraph differs materially from the model answer above, paste it next and I will correct it line by line.
