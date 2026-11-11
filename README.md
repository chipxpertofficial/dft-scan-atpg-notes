# DFT: Scan, ATPG & MBIST Notes

Working notes on **Design-for-Test** — how chips are made testable after manufacturing: scan
chains, ATPG, fault models, test compression, and memory BIST. Written to build intuition for the
one question every DFT interview opens with: *"how do you know a fabricated chip actually works?"*

## Notes

| Note | Topic |
|------|-------|
| [01 · Why DFT, and the fault models](notes/01-why-dft.md) | controllability/observability, stuck-at, transition |
| [02 · Scan & ATPG](notes/02-scan-atpg.md) | scan insertion, the scan shift/capture cycle, coverage |
| [03 · Compression & MBIST](notes/03-compression-mbist.md) | test-data volume, EDT-style compression, memory test |

## The core idea in one paragraph
You can't probe internal flip-flops on a fabricated chip. **Scan** solves this: stitch all flops
into shift registers so you can *shift in* any internal state and *shift out* what the logic
produced. **ATPG** then computes the input patterns that expose manufacturing defects, and the
**fault coverage** number tells you what fraction of possible defects those patterns catch.

## Why it matters commercially
Test cost and quality are real money: low coverage → defective chips reach customers (field
returns); huge pattern sets → long, expensive tester time. DFT is the engineering of that
trade-off — coverage vs test time vs silicon area.

## Companion material
- A research project on the coverage/area/time trade-off → [mtech-vlsi-projects #02](https://github.com/chipxpertofficial/mtech-vlsi-projects)
- DFT interview questions → [vlsi-interview-prep](https://github.com/chipxpertofficial/vlsi-interview-prep)
- Mentored DFT training → [ChipXpert](https://chipxpert.in/)

## License
MIT — see [LICENSE](LICENSE). Maintained by the ChipXpert team.
