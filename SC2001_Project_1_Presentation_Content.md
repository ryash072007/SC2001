# SC2001 Project 1 — Presentation Slide Source

> **Purpose:** Source-of-truth content for building the presentation deck.  
> **Use:** Build the slides from this file and the existing assets in `plots/`.  
> **Important:** Do not substitute numerical results, thresholds, timings, or graph filenames with values from example decks.

---

# Global deck formatting

- **Aspect ratio:** 16:9 widescreen.
- **Style:** clean technical presentation; modern, minimal, high contrast.
- **Background:** light/neutral or very dark, but keep one consistent theme throughout.
- **Typography:** one sans-serif family; bold section titles; no decorative fonts.
- **Title size:** ~30–36 pt.
- **Body size:** ~20–24 pt.
- **Table text:** ~18–20 pt minimum.
- **Main rule:** one main idea per slide.
- **Body density:** normally 3–5 bullets maximum.
- **Graphs:** large; prefer graph + 2–4 observations instead of dense text.
- **Equations:** render properly, not as plain-text screenshots.
- **Code:** use short pseudocode only; do not paste full notebook code.
- **Numbers:** bold the important values.
- **Do not use:** long paragraphs, huge raw result tables, screenshots of notebook cells, example-deck numbers.
- **Graphs:** use the exact local files under `plots/`.
- **Part labels:** visibly mark `(a)`, `(b)`, `(c)(i)`, `(c)(ii)`, `(c)(iii)`, `(d)` in section titles where appropriate.
- **Footer:** optional small “SC2001 Project 1” + slide number.
- **Timing target:** approximately 8 minutes of presentation content, leaving ~2 minutes for Q&A.

---

## Slide 1 — SC2001 Project 1: Hybrid Merge/Insertion Sort

### On-slide text

**SC2001 Algorithm Design & Analysis**  
# Project 1: Integration of Merge Sort & Insertion Sort

**Hybrid Merge/Insertion Sort**

[Team member names]

### Visual / placement

- Large title on the left or center.
- Small abstract recursion-tree graphic on the right:
  - Merge Sort recursion
  - stop at threshold `S`
  - leaf blocks handled by Insertion Sort
- No result graph on this slide.

### Emphasis

Highlight **Hybrid Merge/Insertion Sort** and **threshold S**.

---

## Slide 2 — Why Hybridize Merge Sort?

### On-slide text

### Merge Sort
- **Θ(n log n)** scaling
- Efficient and predictable for large arrays
- But recursively sorting tiny subarrays creates overhead

### Insertion Sort
- Simple, low-overhead inner loop
- Effective on small subarrays
- Poor choice for large arrays because work grows quadratically

### Hybrid idea
> Use Merge Sort for the large-scale structure, then switch to Insertion Sort when subarray size becomes small.

### Visual / placement

Two-column comparison:

**Left:** Merge Sort  
**Right:** Insertion Sort

Across the bottom, show:

`Merge Sort → small subarray → Insertion Sort`

---

## Slide 3 — Threshold S: The Key Design Choice

### On-slide text

# Threshold rule

**Subarray size > S**  
→ continue Merge Sort recursion

**Subarray size ≤ S**  
→ switch to Insertion Sort

### Small S
- deeper recursion
- more merge operations
- smaller insertion-sort leaves

### Large S
- shallower recursion
- fewer merge levels
- larger insertion-sort leaves

### Visual / placement

Use a side-by-side recursion illustration:

**Small S:** deeper tree, many small leaves  
**Large S:** shallower tree, fewer larger leaves

Add a simple horizontal “S” slider between them.

---

## Slide 4 — (a) Hybrid Algorithm

### On-slide text

```text
HybridMergeSort(A, left, right, S):

    size = right - left + 1

    if size <= S:
        InsertionSort(A, left, right)
        return

    mid = (left + right) // 2

    HybridMergeSort(A, left, mid, S)
    HybridMergeSort(A, mid + 1, right, S)

    Merge(A, left, mid, right)
```

**Key comparison:** a comparison between two data values during sorting.

### Visual / placement

- Pseudocode: left ~55%.
- Recursion tree: right ~45%.
- In recursion tree, label terminal blocks **Insertion Sort**.

### Emphasis

Highlight the line:

`if size <= S → Insertion Sort`

---

## Slide 5 — Implementation & Correctness

### On-slide text

### Main implementation
- `hybrid_merge_sort(values, threshold)`
- `merge_sort(values)`
- `merge(...)`
- `insertion_sort_range(...)`
- `make_dataset(n, seed)`

### Correctness validation
- empty and single-element arrays
- sorted and reverse-sorted arrays
- duplicate values
- random input
- **S = 1** follows the same recursion/merge structure as original Merge Sort

> Correctness checks passed before running the experiments.

### Visual / placement

Left: small function map.

Right: checklist with checkmarks.

Bottom callout:

**S = 1 sanity check: same sorted result and same comparison count as Merge Sort**

---

## Slide 6 — (b) Input Generation & Experimental Setup

### On-slide text

### Dataset generation
- random seed: **2001**
- values sampled from **[1, 10,000,000]**
- input sizes extend from **1,000 to 10,000,000**
- same dataset reused within each controlled comparison

### Timing methodology
- CPU timer: `time.process_time_ns()`
- copy input **before** timed section
- time **only the sorting call**
- validate sorted output **after** timed section

### Environment
- Python **3.12.10**
- Windows 11
- process timer implementation: `GetProcessTimes()`

### Visual / placement

Horizontal pipeline:

`Generate → Copy → Sort + count/time → Validate`

Keep the three setup blocks compact.

---

## Slide 7 — Theoretical Complexity Model

### On-slide text

### Merge levels above the cutoff

\[
\Theta\left(n\log_2\frac{n}{S}\right)
\]

### Insertion Sort at the leaves

There are approximately \(n/S\) leaf blocks.

\[
\frac{n}{S}\cdot\Theta(S^2)=\Theta(nS)
\]

### Reference model

\[
\boxed{
T(n,S)=\Theta\left(n\log_2\frac{n}{S}+nS\right)
}
\]

### If S is fixed

\[
\boxed{T(n,S)=\Theta(n\log n)}
\]

### Visual / placement

Left ~45%:
- simplified recursion tree stopping at blocks of size `S`

Right ~55%:
- equations

### Small footnote

**This is an asymptotic reference model, not an exact formula for measured key comparisons.**

---

## Slide 8 — (c)(i) Fixed S, Vary Input Size n

### On-slide text

### Experiment
- fixed threshold: **S = 16**
- vary \(n\): **1,000 → 10,000,000**
- measure: **exact key comparisons**

### Theoretical expectation

For fixed \(S\):

\[
T(n,S)=\Theta(n\log n)
\]

### Empirical test

Check whether:

\[
\frac{\text{comparisons}}{n\log_2n}
\]

remains approximately stable as \(n\) grows.

### Visual / placement

Left ~35%: setup + equation.  
Right ~65%: small conceptual “expected n log n growth” sketch or just whitespace leading into the next slide.

Do **not** put the main result graph here; reserve it for Slide 9.

---

## Slide 9 — (c)(i) Empirical Result

### On-slide text

### Observed
- \(n=1,000\) → **10,413** comparisons
- \(n=10,000,000\) → **226,420,940** comparisons
- normalized ratio stays roughly **0.95–1.04**

### Takeaway

> With **S = 16** fixed, empirical comparison growth is consistent with proportional **\(n\log n\)** behaviour.

### Visual / placement

**Use graph large:**

`plots/c1_comparisons_vs_n.png`

Graph: ~70% of slide.  
Text: ~30%.

### Important

Use both panels already present in the graph:
- comparisons vs \(n\)
- comparisons / \((n\log_2n)\)

Do **not** claim “the graph is linear, therefore O(n log n).”

---

## Slide 10 — (c)(ii) Fixed n, Vary Threshold S

### On-slide text

### Experiment
- fixed input size: **n = 200,000**
- tested \(S\): **2, 4, 8, 16, 32, 64**
- same unsorted dataset for every threshold
- measure: **exact key comparisons**

### Expected trade-off

**Small S**
- more recursive / merge work
- smaller insertion-sort leaves

**Large S**
- recursion stops earlier
- larger insertion-sort leaves
- insertion-sort comparison cost increases

### Visual / placement

Two-column trade-off diagram.

At bottom:

`small S ←──────── threshold S ────────→ large S`

---

## Slide 11 — (c)(ii) Comparison Results

### On-slide text

| S | Key comparisons |
|---:|---:|
| **2** | **3,272,851** |
| 4 | 3,273,283 |
| 8 | 3,315,694 |
| 16 | 3,478,396 |
| 32 | 3,922,280 |
| 64 | 4,962,322 |

### Key observations
- fewest comparisons: **S = 2**
- comparisons increase strongly as \(S\) becomes large
- \(S=64\) performs about **51.6% more comparisons** than \(S=2\)

### Takeaway

> Increasing S too far shifts too much work to Insertion Sort.

### Visual / placement

**Right ~60%:**

`plots/c2_comparisons_vs_s.png`

**Left ~40%:**
- compact table
- 2–3 observations

---

## Slide 12 — (c)(iii) Finding a Practical Threshold

### On-slide text

# “Optimal S” depends on the objective

### Objective 1 — Key comparisons
Minimize the number of direct data-value comparisons.

### Objective 2 — CPU time
Minimize practical execution cost.

These do **not** necessarily select the same threshold.

### Experiment
- \(n\): **100,000; 500,000; 1,000,000; 5,000,000**
- \(S\): **2, 4, 8, 12, 16, 20, 24, 32, 48, 64**
- **5 CPU-time measurements** per \((n,S)\)
- use **median CPU time**

### Visual / placement

Top: two large objective cards.

Bottom: compact experiment configuration.

---

## Slide 13 — (c)(iii) Comparison-Count Trade-off

### On-slide text

### Best S by key comparisons

| n | Best S | Comparisons |
|---:|---:|---:|
| 100,000 | **2** | 1,536,133 |
| 500,000 | **2** | 8,837,346 |
| 1,000,000 | **2** | 18,675,172 |
| 5,000,000 | **2** | 105,050,548 |

### Observation

> Across every tested input size, **S = 2** produced the fewest key comparisons.

Larger \(S\) values increase insertion-sort work and therefore increase comparison counts.

### Visual / placement

**Use/crop LEFT panel only:**

`plots/c3_threshold_tradeoff.png`

Graph ~65%, table/text ~35%.

---

## Slide 14 — (c)(iii) CPU-Time Trade-off

### On-slide text

### Fastest measured median CPU time

| n | Fastest measured S | Median CPU time |
|---:|---:|---:|
| 100,000 | **16** | **0.109375 s** |
| 500,000 | **8** | **0.687500 s** |
| 1,000,000 | **8** | **1.515625 s** |
| 5,000,000 | **12** | **10.390625 s** |

### Observation

> The fastest measured region is at **moderate S**, not at the comparison-minimizing \(S=2\).

Moderate thresholds reduce recursion/merge overhead without making insertion-sort blocks too large.

### Visual / placement

**Use/crop RIGHT panel only:**

`plots/c3_threshold_tradeoff.png`

Graph ~65%, table/text ~35%.

### Small footnote

Where multiple thresholds tied for the minimum median time, the notebook reports the **first tested S** at that minimum.

---

## Slide 15 — Why Use Median Timing?

### On-slide text

### Timing measurements fluctuate

Potential sources:
- OS scheduling
- background processes
- cache / system state
- timer quantization
- execution-order effects

### Our method
- **5 runs** for every \((n,S)\)
- alternate threshold order across repetitions
- report the **median**
- compare thresholds using relative performance within each input size

### Takeaway

> One timing run is too fragile for choosing S.

### Visual / placement

Use a simple five-dot example:

`run 1   run 2   run 3   run 4   run 5 → median`

No notebook graph needed.

---

## Slide 16 — Selecting the Final S

### On-slide text

### Aggregate selection method

For each input size \(n\):

\[
\text{relative time}(n,S)
=
\frac{\text{median time at }S}
{\text{fastest median time for that }n}
\]

Then average the relative times across all tested input sizes.

### Selected values near the optimum

| S | Mean relative median CPU time |
|---:|---:|
| 8 | 1.042105 |
| 12 | 1.038292 |
| **16** | **1.000000** |
| 20 | 1.039098 |
| 24 | 1.003008 |

# Selected practical threshold: **S = 16**

### Visual / placement

**Use graph:**

`plots/c3_mean_cpu_time_penalty_by_s.png`

Graph ~60%.

Selection method + mini-table ~40%.

### Bottom callout

**S = 16 is the best aggregate measured threshold for this implementation and test environment — not a universal mathematical constant.**

---

## Slide 17 — (d) Final Comparison Methodology

### On-slide text

# Original Merge Sort vs Hybrid Sort

### Setup
- \(n = \mathbf{10,000,000}\)
- same dataset for both algorithms
- Hybrid threshold: **S = 16**
- **3 timing runs per algorithm**
- exact key-comparison count recorded

### Counterbalanced execution order

**Merge → Hybrid → Hybrid → Merge → Merge → Hybrid**

### Why counterbalance?
Reduce bias from:
- execution order
- cache/system state
- gradual system-load drift

### Visual / placement

Right ~55%:

`plots/d_10m_raw_cpu_time_by_sequence.png`

Left ~45%:
- setup
- run order
- one-sentence rationale

---

## Slide 18 — (d) Final Results

### On-slide text

| Metric | Original Merge Sort | Hybrid Sort, S = 16 |
|---|---:|---:|
| Key comparisons | **220,098,332** | **226,415,036** |
| Median CPU time | **30.156250 s** | **25.609375 s** |

### Hybrid − Merge
- comparisons: **+6,316,704**
- comparison difference: **+2.870%**
- median CPU-time difference: **−4.546875 s**
- CPU-time reduction: **15.078%**
- speedup: **1.178×**

### Main result

> **More key comparisons — but lower CPU time.**

### Visual / placement

Bottom half split equally:

**Left:**  
`plots/d_10m_comparisons.png`

**Right:**  
`plots/d_10m_median_cpu_time.png`

Top half: compact comparison table + highlighted result.

---

## Slide 19 — Why More Comparisons Can Still Be Faster

### On-slide text

# Key comparisons measure only one part of the work

### Comparison count captures
- comparisons between data values

### CPU time also includes
- recursive function calls
- merge loops
- auxiliary-array copying
- control flow
- memory accesses
- Python function/interpreter overhead

### Interpretation

> In our experiment, the reduction in recursion and merge overhead outweighed the extra key comparisons performed inside the Insertion Sort leaves.

### Visual / placement

Use a balance / trade-off graphic:

**+2.870% key comparisons**  
vs  
**−15.078% median CPU time**

Make the CPU-time result visually dominant.

---

## Slide 20 — Conclusions

### On-slide text

# Key findings

1. **Correct hybrid implementation**  
   Merge Sort switches to Insertion Sort when subarray size ≤ S.

2. **Fixed S preserves Merge Sort-like scaling**  
   Empirical comparisons are consistent with **Θ(n log n)** growth.

3. **S creates a real trade-off**  
   Large S reduces recursion but increases insertion-sort comparison work.

4. **Comparison-optimal ≠ runtime-optimal**  
   Comparisons favored **S = 2**; practical CPU time favored moderate thresholds.

5. **Selected practical threshold: S = 16**

6. **At n = 10,000,000:**  
   Hybrid used **2.870% more comparisons** but **15.078% less median CPU time**  
   → **1.178× speedup**

### Bottom line

> Hybridization improved practical runtime in our experiment by reducing overhead — not by minimizing key comparisons.

### Visual / placement

Use a four-step summary strip:

`Theory → C(i)/(ii) → S = 16 → 1.178×`

Bottom-right:

**Q&A**

---

# Asset map

Use these exact files from `plots/`:

| Slide | Asset |
|---:|---|
| 9 | `plots/c1_comparisons_vs_n.png` |
| 11 | `plots/c2_comparisons_vs_s.png` |
| 13 | `plots/c3_threshold_tradeoff.png` — crop/use LEFT panel |
| 14 | `plots/c3_threshold_tradeoff.png` — crop/use RIGHT panel |
| 16 | `plots/c3_mean_cpu_time_penalty_by_s.png` |
| 17 | `plots/d_10m_raw_cpu_time_by_sequence.png` |
| 18 | `plots/d_10m_comparisons.png` |
| 18 | `plots/d_10m_median_cpu_time.png` |

## Generated graphs that should normally be omitted

- `plots/c1_comparisons_per_element.png`
- `plots/c2_comparison_penalty_by_s.png`
- `plots/c3_cpu_time_penalty_by_s.png`

They are valid results but add less value than the selected visuals for an ~8-minute presentation.

---

# Critical numbers — do not change

- C(i) fixed threshold: **S = 16**
- C(i) range: **n = 1,000 to 10,000,000**
- C(ii) fixed input: **n = 200,000**
- C(ii) tested S: **2, 4, 8, 16, 32, 64**
- C(ii) minimum comparisons: **3,272,851 at S = 2**
- C(iii) n values: **100,000; 500,000; 1,000,000; 5,000,000**
- C(iii) S values: **2, 4, 8, 12, 16, 20, 24, 32, 48, 64**
- C(iii) timing repetitions: **5**
- selected threshold: **S = 16**
- Part D input size: **10,000,000**
- Part D Merge comparisons: **220,098,332**
- Part D Hybrid comparisons: **226,415,036**
- Part D Merge median CPU time: **30.156250 s**
- Part D Hybrid median CPU time: **25.609375 s**
- Part D comparison difference: **+2.870%**
- Part D CPU-time difference: **−15.078%**
- Part D speedup: **1.178×**

---

# Build constraints for the presentation agent

- Treat this Markdown file as the **content source of truth** for the deck.
- Treat the current notebook / generated `plots/` assets as the **data source of truth**.
- Do not import any numerical result from the example presentation PDFs.
- Do not invent or redraw notebook graphs unless explicitly asked.
- Do not use screenshots of notebook outputs if the corresponding PNG graph exists.
- Keep all equations editable/rendered.
- Keep all slide text editable.
- Preserve the exact result values above.
- Do not call **S = 16** universally optimal; call it:
  - **selected practical threshold**
  - **best aggregate measured threshold**
  - or equivalent wording tied to this implementation/test environment.
- Runtime conclusions must be phrased as results **from our experiment**, not universal guarantees.
