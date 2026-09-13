## Xiangyu Deng

M.S. Software Engineering, Carnegie Mellon University (School of Computer Science) — graduating December 2026.

Currently on the **MoonRanger** lunar rover at CMU's Robotics Institute, working on
CUDA stereo depth reconstruction (`libSGM`, `stereo_reconstructor`) in a NASA cFS
flight-software stack. Before that, product work at Tencent on trust and safety
for League of Legends China, and an ICASSP paper on spatial audio quality
assessment as co-first author.

I care about work that is honest about what it measured. Most of what follows is
built around that: masked metrics, fixed protocols, invariants asserted after
every operation, and negative results left in.

---

### Systems & trading infrastructure

**[lob-matching-engine](https://github.com/xdeng3-collab/lob-matching-engine)** — C++17
price-time-priority limit order book and matching engine. Flat tick-indexed price
levels with intrusive FIFOs and an occupancy bitmap: **42 ns p50 / 541 ns p99**
against 125 ns / 9.3 µs for the usual `std::map` of levels, over two million
events. Correctness rests on a property-based suite that asserts book invariants
— never crossed, quantity conserved, FIFO links consistent — after *every* random
operation.

**[lowlat-cpp-bench](https://github.com/xdeng3-collab/lowlat-cpp-bench)** — four
low-latency experiments with measured before-and-after. False sharing costs 4.2×
in isolation; AoS→SoA is worth up to 3.8× once the working set outgrows L2;
acquire-release RMW costs 3.4× over relaxed on AArch64 where x86 gives it away
free. **Two of the four results are negative and stayed in** — padding the queue
indices buys nothing in situ despite the 4.2× the isolated test shows.

**[alpha-factor-lab](https://github.com/xdeng3-collab/alpha-factor-lab)** —
cross-sectional factor research where look-ahead bias is caught by the machinery
rather than by careful reading. `Panel.asof(t)` cannot return a row after *t*; a
truncate-and-compare detector proves it for factors not written that way. The
pipeline calibrates on pure noise before any result is believed, and every IC
ships with turnover, a cost curve, and a breakeven cost.

### Robotics & perception

**[stereo-sgm-benchmark](https://github.com/xdeng3-collab/stereo-sgm-benchmark)** —
stereo disparity benchmarking with metrics masked twice, by ground-truth validity
and by whether the matcher estimated anything at all. Synthetic pairs whose truth
is exact by construction, including forward-warp occlusion. A 144-setting sweep
shows 8-path aggregation buying the last 20% of accuracy for 37% more time, and
nothing else on the grid reaching it at any price.

**[vla-eval-harness](https://github.com/xdeng3-collab/vla-eval-harness)** —
evaluation for robot policies. Protocols are fingerprinted, so comparing results
produced under different step limits raises instead of quietly returning a
difference. Success rates carry Wilson intervals; comparisons are exact paired
McNemar tests, which show that **winning 12 episodes and losing 4 is p = 0.077**
and that 50 episodes cannot resolve anything under ~20 points. LeRobot adapters
included.

**[mujoco-rl-locomotion](https://github.com/xdeng3-collab/mujoco-rl-locomotion)** —
composable reward terms plus a detector for the failure reward curves cannot show.
Removing the smoothness term leaves the return *identical* while distance falls
85% and action chatter rises sixfold — a policy scoring well without walking.

**[ros2-nav-sandbox](https://github.com/xdeng3-collab/ros2-nav-sandbox)** —
differential-drive description and Nav2 config, with a dependency-free TF tree
validator in CI. A malformed transform tree is the most common reason a
navigation stack sits still *while reporting no error*; every failure it must
catch lives in `urdf/broken/` and the suite asserts it gets rejected.

### Products

**[capwords](https://github.com/xdeng3-collab/capwords)** — iOS app: photograph an
object, learn the word. React Native + Expo, Supabase, a native StoreKit 2 module,
a home-screen widget, and a design system rendered from code-authored pixel grids
with zero image assets. Four pricing tiers modelled against a ~$0.0025/word
marginal cost.

---

Pittsburgh, PA · [LinkedIn](https://www.linkedin.com/in/xiangyu-deng/) · xdeng3@andrew.cmu.edu
