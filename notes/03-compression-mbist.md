# 03 · Test compression & memory BIST

Two more pieces complete a real DFT story: **compression** (making scan affordable) and **MBIST**
(testing the memories scan can't).

## The test-data-volume problem
Scan works, but patterns for a big design are huge — too much data to store on the tester and too
many shift cycles (test time = money). **Test compression** attacks both.

## How compression works (EDT-style)
Insert a **decompressor** between a small number of tester channels and the many internal scan
chains, and a **compactor** on the output side:
```
tester (few channels) → decompressor → many short scan chains → compactor → tester
```
- One tester channel feeds many chains → **fewer shift cycles** (chains are shorter).
- The compactor squeezes many chain outputs into few tester pins.
Compression ratios of 10×–100× are routine. The trade: a little extra logic, and the compactor can
mask some faults (handled by X-tolerant designs).

## The compression trade-off (worth a research project)
Higher compression → less test data and time, but diminishing coverage returns and more
on-chip logic. Plotting coverage/time/area vs ratio is exactly
[mtech-vlsi-projects #02](https://github.com/chipxpertofficial/mtech-vlsi-projects).

## Why memories need their own test (MBIST)
Scan tests logic, not RAM arrays — memories have their own defect modes (stuck cells,
coupling between adjacent cells, address-decoder faults). **Memory BIST** puts a small on-chip
controller next to each RAM that runs **March algorithms**.

## March tests
A March test is a sequence of read/write operations swept up and down the address space. Example,
**March C-** (common baseline):
```
⇕(w0); ⇑(r0,w1); ⇑(r1,w0); ⇓(r0,w1); ⇓(r1,w0); ⇕(r0)
```
Each element detects a class of faults (stuck-at, transition, coupling). Stronger algorithms
(March SS, March LR) catch more coupling faults at the cost of more cycles.

## Putting it together — a chip's test suite
1. **Scan + ATPG** (compressed) for the logic → stuck-at + transition coverage.
2. **MBIST** for every RAM → March-based memory coverage.
3. **Boundary scan (JTAG, IEEE 1149.1)** for board-level interconnect.
Report each coverage number **separately** — a single blended "test coverage" hides which part is
weak.
