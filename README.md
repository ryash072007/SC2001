# SC2001 Project 1 - Hybrid Merge Sort

The executed notebook [`SC2001_Project_1_Hybrid_Sort.ipynb`](SC2001_Project_1_Hybrid_Sort.ipynb) is the primary lab record. It contains the textbook-style algorithms, theoretical analysis, experimental methodology, tables, plots, interpretation, and direct audit against parts (a)-(d).

## Corrected experiment configuration

- Python 3.13.5 on Windows
- Seed: `2001`
- Random integer range: `[1, 10_000_000]`
- CPU timer: `time.process_time_ns()` (`GetProcessTimes()` on this machine)
- Part (c)(i): `S = 16`; 13 sizes from 1,000 through 10,000,000
- Part (c)(ii): `n = 200_000`; `S = 1, 2, 4, 8, 16, 32, 64, 128, 256`
- Part (c)(iii): `n = 100_000, 500_000, 1_000_000, 2_000_000`; `S = 2, 4, 8, 12, 16, 20, 24, 32, 48, 64`; five CPU runs per pair
- Part (d): one shared 10,000,000-integer dataset; three runs per algorithm in order Merge, Hybrid, Hybrid, Merge, Merge, Hybrid
- Input copying and output verification occur outside the timed sorting interval

## Findings

- Part (c)(ii) comparison minimum: `S = 1` and `S = 2` tied at 3,272,851 comparisons.
- Part (c)(iii) comparison optimum over its tested range: `S = 2`.
- Part (c)(iii) per-size measured minima, using the first minimum where quantized medians tie: `S = 8, 12, 12, 20` for `n = 100,000, 500,000, 1,000,000, 2,000,000`.
- Broad near-optimal practical timing region under the declared 3% aggregate rule: approximately `S = 8-24` (`S = 8, 12, 16, 20, 24` were included).
- Selected part (d) threshold: `S = 16`, a reasonable central representative of that region, not a uniquely or exactly optimal value.
- Original Merge Sort at 10 million: 220,098,332 comparisons; CPU runs 60.187500, 60.953125, 60.359375 s; median 60.359375 s.
- Hybrid Sort at 10 million with `S = 16`: 226,415,036 comparisons; CPU runs 54.281250, 53.578125, 53.328125 s; median 53.578125 s.
- Hybrid difference: +6,316,704 comparisons (+2.870%) and -6.781250 median CPU seconds (-11.235%); Merge/Hybrid median-time ratio 1.127x.

The hybrid speedup did not result from fewer comparisons: it performed more comparisons. CPU time also reflects ordinary implementation overhead, and the hybrid avoids some recursive Merge Sort and small-merge overhead by using Insertion Sort on small subarrays.

The effective Windows process CPU clock remained quantized at 0.015625 s despite using the nanosecond API. Larger trials, five-run medians, and the 3% near-tie rule make the practical conclusion more defensible without pretending the clock quantum disappeared.

## Presentation assets

- `plots/c1_comparisons_vs_n.png` - required fixed-S comparison scaling
- `plots/c2_comparisons_vs_s.png` - required fixed-n threshold comparison
- `plots/c3_threshold_tradeoff.png` - comparison and CPU-time threshold trade-off
- `plots/d_10m_median_cpu_time.png` - final 10-million-element median CPU comparison
- `results/c1_fixed_s_varying_n.csv` - part (c)(i) raw table
- `results/c2_fixed_n_varying_s.csv` - part (c)(ii) raw table
- `results/c3_optimal_s_raw.csv` - every part (c)(iii) CPU run and comparison count
- `results/c3_threshold_summary.csv` - aggregate threshold selection
- `results/d_10m_raw_runs.csv` - all six counterbalanced part (d) runs
- `results/d_10m_summary.csv` - part (d) medians and differences

## Project-requirement audit

- **(a):** Hybrid algorithm uses insertion sort exactly when the recursive subarray size is at most `S`; both sorts share the same merge and auxiliary-array design.
- **(b):** Increasing seeded random datasets cover 1,000 through 10,000,000 integers.
- **(c)(i):** Fixed-S key comparisons are plotted against `n` and compared with the expected fixed-S `Theta(n log n)` behaviour.
- **(c)(ii):** Fixed-n key comparisons are plotted against `S` and interpreted using `Theta(n log(n/S) + nS)`.
- **(c)(iii):** Multiple larger input sizes, non-power-of-two thresholds, and repeated median CPU times support an optimal region rather than a false single-point optimum.
- **(d):** Both algorithms are compared on exactly 10,000,000 identical integers using exact key comparisons and three counterbalanced CPU runs each.
