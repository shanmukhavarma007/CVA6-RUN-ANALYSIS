# CVA6 Physical Design – Iteration & Run Analysis

A standalone engineering log documenting physical-design experiments on the **CVA6 RISC-V design**. This repository focuses on the reasoning behind each run: observed timing and congestion, floorplan/placement changes, CTS and routing outcomes, and the decisions taken for subsequent experiments.

> **Source of truth:** the three original DOCX reports are preserved in [`docs/original-reports/`](docs/original-reports/). The Markdown pages are a navigable presentation of those reports. Where a source report does not provide a numerical result or a complete explanation, this repository says so rather than filling the gap.

## Repository map

- [`iterations/`](iterations/) – one page per documented experiment / stage.
- [`docs/original-reports/`](docs/original-reports/) – original reports, retained unchanged.
- [`evidence/`](evidence/) – reserved for exported screenshots, congestion maps, timing reports, and other visual evidence.

## Iteration index

| Page | Experiment / stage | Main question |
|---|---|---|
| [00 – Baseline diagnosis](iterations/00-baseline-diagnosis.md) | Initial placement diagnosis and cluster-based placement proposal | Are long register-to-register paths in `ex_stage_i` driving timing issues? |
| [01 – Default placement through route](iterations/01-default-placement-cts-route.md) | Default placement, CTS, post-CTS, route and post-route cleanup | How does the default floorplan behave through implementation? |
| [02 – Floorplan and congestion-effort exploration](iterations/02-floorplan-and-congestion-effort.md) | Floorplan dimension change and higher congestion/wire-length effort | Can placement congestion be improved without sacrificing timing? |
| [03 – Additional macro channel](iterations/03-additional-macro-channel.md) | Move eight macros by 5 µm; retain high congestion and wire-length effort | Does extra macro-channel space reduce local congestion? |
| [04 – Soft blockage and clock buffering](iterations/04-soft-blockage-clock-buffering.md) | Soft blockage, 15 µm channel gap, stronger clock buffering | Can routing resources and clock-path drive be improved? |
| [05 – `ex_stage_i` module padding](iterations/05-ex-stage-module-padding.md) | Module padding with the extra-channel floorplan | Does keeping `ex_stage_i` cells together improve timing? |
| [06 – AOI cell padding](iterations/06-aoi-cell-padding.md) | Padding AOI cells in `ex_stage_i` | Does targeted cell padding improve timing? |
| [07 – Placement density settings](iterations/07-placement-density-settings.md) | Max density 0.6 and global place density enabled | Does changing global placement density improve congestion? |
| [08 – Physical-aware synthesis exploration](iterations/08-physical-aware-synthesis.md) | Proposed physical-aware synthesis | Can synthesis account for physical effects earlier? |

## Key observations captured in the reports

- The initial baseline used a **950 × 916 µm** floorplan (reported area **850,552 µm²**), a **20 µm** channel between two macro hierarchies, and **30% partial placement blockage** between macros. The placement report recorded **82.6% core utilization**, **0.2% overflow**, and **1.56 hotspot score**.
- The default-flow report records a successful placement handoff to CTS, but the CTS report identifies transition violations as a blocker despite improved congestion/hotspot behavior relative to earlier runs.
- The run-3 floorplan experiment was selected for its timing and placement congestion results, but its CTS result was described as congestion-heavy near macros, particularly around `ex_stage_i`.
- The later experiments tested macro spacing, blockage type, clock buffering, module/cell padding, and placement-density controls. The source records no major timing improvement for iteration 6 and no improvement for iteration 7.
- The route/timing-fix report records **0 DRC violations** after a route ECO and manual wide-metal-jog cleanup, then reports positive WNS values for the I2O and default path groups after constraint adjustments.

## Important interpretation notes

1. **Iteration numbering follows the source reports.** The documents use both “Run” and “Iteration” labels and include an “Iteration -1” baseline. The index above separates the initial diagnosis from the numbered experiments for readability.
2. **Do not compare unlike stages as if they were the same checkpoint.** Placement, CTS, post-CTS, route, and post-route metrics are different stages.
3. **Some timing values are embedded only in screenshots.** Consult the original DOCX reports for the full timing tables and visual heatmaps.
4. **Iteration 8 is a decision/proposal in the source report.** No physical-aware synthesis result is supplied in the uploaded material.
5. This repository documents the supplied reports; it does not claim signoff, tapeout, or a fully closed timing/congestion result beyond what the reports explicitly state.

## Original reports

- `01_cluster_based_placement_analysis.docx`
- `02_run1_cts_route_and_timing_fixes.docx`
- `03_floorplan_and_placement_iterations_2_to_8.docx`

## Suggested GitHub presentation

Set the repository description to:

> CVA6 RISC-V physical-design run analysis covering floorplanning, placement optimization, CTS, routing, congestion analysis, DRC cleanup, and timing fixes across iterative experiments.

Suggested topics: `cva6`, `risc-v`, `physical-design`, `asic`, `floorplanning`, `placement`, `cts`, `routing`, `timing-closure`.
