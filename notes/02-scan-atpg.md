# 02 · Scan & ATPG

## Scan — turning flops into a shift register
Each functional flip-flop is replaced by a **scan flop** (a flop with a mux on its D input). In
**scan mode**, the flops chain into one or more long shift registers (**scan chains**); in
**functional mode**, they behave normally.

This gives you:
- **Controllability** — shift any state you want *into* every flop.
- **Observability** — shift the captured state *out* to a pin.

## The scan test cycle
For each test pattern:
1. **Scan-in** — with `scan_enable = 1`, shift the desired state into the chain (takes as many
   clocks as the chain is long).
2. **Capture** — set `scan_enable = 0`, pulse the clock once; the combinational logic computes and
   the result is captured into the flops.
3. **Scan-out** — with `scan_enable = 1`, shift the captured result out (overlapped with scan-in of
   the next pattern).

Shift dominates test time, which is why chain length and chain count matter.

## ATPG — computing the patterns
**Automatic Test Pattern Generation** takes the gate netlist + fault list and computes, for each
fault, a pattern that:
- **activates** the fault (drives the node to the opposite of its stuck value), and
- **propagates** its effect to an observable (scan) point.
Modern ATPG also **compacts** patterns so one pattern detects many faults, shrinking the set.

## At-speed (transition) testing
Transition faults need **two** capture clocks at functional speed:
- **Launch-off-capture** or **launch-off-shift** creates the 0→1/1→0 transition, then captures one
  cycle later to see if it arrived in time.
This needs the real clock to be pulsed at speed during capture — more complex than static
stuck-at, but it catches timing defects.

## Reading ATPG results
- **Test coverage / fault coverage** — the headline (aim high for stuck-at; report transition
  separately).
- **Pattern count** — drives test time; compression (next note) attacks this.
- **ATPG-untestable (ATPG_U)** — faults ATPG proved can't be detected (often redundant logic); not
  a failure, but understand why.

## DFT rule checks (why your scan insertion fails)
Before scan works, the design must pass DRC-for-test: no uncontrollable clocks, no combinational
loops, resets controllable in test mode, no un-scannable flops on the chain. Fixing these is most
of the real DFT work.

Next: [compression & MBIST](03-compression-mbist.md).
