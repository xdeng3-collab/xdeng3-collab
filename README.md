## Xiangyu Deng

M.S. Software Engineering, Carnegie Mellon University (School of Computer Science) — graduating December 2026.

Currently on the **MoonRanger** lunar rover at CMU's Robotics Institute, working on
CUDA stereo depth reconstruction (`libSGM`, `stereo_reconstructor`) in a NASA cFS
flight-software stack. Before that, product work at Tencent on trust and safety
for League of Legends China, and an ICASSP paper on spatial audio quality
assessment as co-first author.

I care about work that is honest about what it measured. Most of what follows is
built around that: masked metrics, fixed protocols, invariants asserted after
every operation, and negative results left in. Where a number depends on the
machine it was taken on, it says so and shows the second machine.

---

### Shipped

**[capwords](https://github.com/xdeng3-collab/capwords)** — iOS app: photograph an
object, learn the word. React Native + Expo SDK 52, ~5,500 lines, built and
iterated over a month.

The part I would defend in a review is the **design system**: every sprite,
icon, and UI element is rendered from code-authored pixel grids as React Native
Views — the pet, its outfits, the sticker art, the whole Stardew-Valley-styled
interface — with **zero image assets** in the bundle. Sprites are data, so a new
outfit is a grid literal rather than an artist round-trip.

The other part is the **pricing model**, worked backwards from measured unit
cost: $0.0025 per word all-in (API input, API output, storage), against four
tiers — free at 3 words/day, $0.01/word packs, $4.99/month, $39.99/year. The
question that drove it is which tier a heavy user should be pushed toward before
they become unprofitable.

Status: **prototype**. State lives in AsyncStorage on the device, so the social
features in the README are designed and not yet backed by a server, and billing
is modelled rather than integrated.

---

### Trading infrastructure & quantitative research

**[lob-matching-engine](https://github.com/xdeng3-collab/lob-matching-engine)** — C++17
price-time-priority limit order book and matching engine. Flat tick-indexed price
levels with intrusive FIFOs and an occupancy bitmap: **165 ns/op against 2,614 ns
for the usual `std::map` of levels**, a 15.8× median over two million events, with
the tail at 708 ns p99 against 20.7 µs. Correctness rests on a property-based
suite that asserts book invariants — never crossed, quantity conserved, FIFO
links consistent — after *every* random operation.

The benchmark measures its own clock first. `steady_clock` here advances in 41 ns
steps and a `now()` pair costs 42 ns, so per-operation timing cannot report a
median in that range — which is why cost is measured amortised, over the whole
replay, and the per-operation table is used only for the tail. It also repeats
and prints the range: one invocation saw the speedup swing 3.4× to 20.7× while
the median held at 15. A benchmark that reports one number from one run is
reporting the scheduler.

**[alpha-factor-lab](https://github.com/xdeng3-collab/alpha-factor-lab)** —
cross-sectional factor research where look-ahead bias is caught by the machinery
rather than by careful reading. `Panel.asof(t)` cannot return a row after *t*; a
truncate-and-compare detector proves it for factors not written that way, and a
deliberately leaking factor is carried in the suite so the detector is tested
against something it must catch. The pipeline calibrates on pure noise before any
result is believed — every factor must come back under |IC| 0.02 — and every IC
ships with turnover, a cost curve, and a breakeven cost. CI asserts those numbers
rather than just running the commands.

---

### Robotics & perception

**[stereo-sgm-benchmark](https://github.com/xdeng3-collab/stereo-sgm-benchmark)** —
stereo disparity benchmarking with metrics masked twice, by ground-truth validity
and by whether the matcher estimated anything at all. Synthetic pairs whose truth
is exact by construction, including forward-warp occlusion. A 144-setting sweep
shows **8-path aggregation is the only thing on the grid that reaches MAE 0.220**;
the best any 5-path setting manages is 0.280.

Re-run on a second machine, every accuracy figure reproduces bit-identically and
none of the timings do — the premium 8-path charges for that accuracy is +34% on
one machine and +17% on the other. So the transferable claim is the accuracy one,
and a latency budget has to be measured on the target rather than read off
someone else's millisecond column. Both runs are committed, each stamped with the
machine that produced it.

**[policy-eval-statistics](https://github.com/xdeng3-collab/policy-eval-statistics)** —
statistics for robot-policy evaluation. Protocols are fingerprinted, so comparing
results produced under different step limits raises instead of quietly returning a
difference. Success rates carry Wilson intervals; comparisons are exact paired
McNemar tests, which show that **winning 12 episodes and losing 4 is p = 0.077**
and that a 20-point gap needs 93 episodes — so the 50 that usually get reported
cannot resolve it. Runs on built-in baselines with no GPU and no simulator; the
LeRobot adapters are typed seams, not integrations, and the README says so.

---

Pittsburgh, PA · [LinkedIn](https://www.linkedin.com/in/xiangyu-deng/) · xdeng3@andrew.cmu.edu
