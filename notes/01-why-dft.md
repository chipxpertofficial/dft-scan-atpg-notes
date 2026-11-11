# 01 · Why DFT, and the fault models

## The problem
A fabricated chip has millions of internal flip-flops you can't probe. A manufacturing defect (a
shorted wire, an open via, a weak transistor) may leave the design *logically correct* but
*physically broken*. You need to detect those defects at the tester — using only the chip's pins.

## Controllability & observability
Two properties decide testability:
- **Controllability** — can you *set* an internal node to a chosen value from the pins?
- **Observability** — can you *see* an internal node's value at the pins?
Deep internal logic scores poorly on both. DFT exists to improve them cheaply.

## Fault models — what we test *for*
We don't test every physical defect directly; we test against **models** that stand in for them:

### Stuck-at
A node is permanently stuck at 0 (SA0) or 1 (SA1). The workhorse model — simple, and a high
stuck-at coverage correlates well with catching real opens/shorts. "Stuck-at coverage 99%" is the
number everyone quotes.

### Transition (at-speed)
A node is *slow* to transition 0→1 or 1→0 — it eventually gets there, but not within the clock
period. Catches timing-related defects that stuck-at misses. Requires **at-speed** patterns (two
clocks: launch then capture), not just static ones.

### Others (good to name)
- **Bridging** — two nodes shorted together.
- **IDDQ** — detects abnormal quiescent current (shorts) by measuring supply current.
- **Path delay** — targets specific critical paths being too slow.

## Fault coverage — the headline metric
```
coverage = detected faults / total testable faults
```
- High stuck-at coverage (≥ 99% is typical for logic) → few defective chips escape.
- But coverage ≠ 100% is normal — some faults are **untestable** (redundant logic, tied nodes).
- Transition coverage is usually a bit lower than stuck-at; **report them separately**, never
  conflate them.

## The trade-off DFT engineers manage
More coverage → more patterns → longer (costlier) tester time, and scan adds area. The craft is
hitting the coverage target at acceptable area and test time — the subject of
[mtech-vlsi-projects #02](https://github.com/chipxpertofficial/mtech-vlsi-projects).

Next: [scan & ATPG](02-scan-atpg.md).
