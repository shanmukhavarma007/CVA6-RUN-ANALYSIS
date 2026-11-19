# 01 – Default Placement, CTS, Route and Timing Fixes

## Initial floorplan and placement

The report specifies:

| Parameter | Reported value |
|---|---:|
| Floorplan dimensions | 950 × 916 µm |
| Area | 850,552 µm² |
| Macro channel between two stacked hierarchies | 20 µm |
| Blockage between macros | Partial blockage, 30% density |
| Placement strategy | Default settings |
| Core utilization | 82.6% |
| Congestion overflow | 0.2% |
| Hotspot score | 1.56 |

The placement heatmap is followed by the verdict that the database could safely move forward to CTS.

## CTS – Iteration 1

Using placement database `cva6_place.db`:

- Core utilization: **83.2%**
- Overflow: **0.47%** (horizontal 0.11%, vertical 0.36%)
- Hotspot score: **14.83**

The report says timing was slightly higher than in previous runs, while congestion and hotspot results were substantially better. It nevertheless identifies maximum-transition violations as too high for proceeding to later stages.

## Post-CTS – Iteration 1

Using `cva6_cts.db`:

- Core utilization: **83.6%**
- Overflow: **0.53** (horizontal 0.11%, vertical 0.44%)
- Hotspot score: **16.39**

Setup and hold reports and a congestion heatmap are included in the source DOCX.

## Route and post-route – Iteration 1

Using `cva6_pcts.db` at route:

- Core utilization: **100%**
- DRC count: **40**
- Hotspot: **18.12**
- Overflow: **0.51** (horizontal 0.20%, vertical 0.31%)
- Antenna violations: **0**

Using `cva6_route.db` post-route:

- Core utilization: **100%**
- DRC count: **10**
- Congestion overflow: **0.51%**
- Hotspot: **18.12**

The report then documents timing analysis before and after enabling LVT cells, and after useful-skew analysis. Full timing tables are preserved in the source report screenshots.

## Route ECO and DRC cleanup

After rerunning route ECO with `route_via_with_block_pins_only true`, the report records:

- DRC update: **0 violations**
- Wide-metal-jog (WMJ) issues were manually cleared by replacing and rotating vias over macro pins.

## Timing fixes

The report identifies two path groups:

| Path group | Reported status | WNS | Approach |
|---|---|---:|---|
| I2O group | Timing fixed | +0.028 ns | Inserted a maximum-delay value |
| Default group | Timing fixed | +0.186 ns | Reduced I/O delay budget from 60% to 40% of the clock period |

The source explains that I2O paths lacked valid timing endpoints without a maximum-delay constraint. For default paths, it describes reset-port-to-register-CDN paths and recovery/removal checks as the reason for adjusting the I/O delay budget.

## Source

`docs/original-reports/02_run1_cts_route_and_timing_fixes.docx` (11 pages; contains congestion heatmaps, timing tables, DRC summaries, and route ECO evidence).
