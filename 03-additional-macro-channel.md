# 03 – Additional 5 µm Macro Channel (Run 3)

## Design changes

The report describes adding another **5 µm** of channel space near the congested macro region by moving **eight macros 5 µm upward**.

- Updated floorplan database path recorded in the report: `proj/final_project/cva6/run2/pnr/floorplan/outputs/cva6_fp1.db`
- Placement settings: high congestion effort and high detail wire-length optimization effort.

## Reported results

- Core utilization: **82.6%**
- Congestion and timing reports are included as screenshots.

The report later selects the run-3 database, describing it as having optimal timing and one of the best congestion-overflow results among the runs completed at that point.

## CTS observation on run-3 database

After initial CTS (`ccopt`) on the run-3 database:

- Core utilization after CTS: **82.7%**
- The report characterizes the design as meeting CTS handoff timing criteria based on the available evidence.
- It also describes the design as congestion-heavy near macros, mostly around the `ex_stage_i` module.

**Engineering takeaway:** The extra channel and placement-effort combination was selected for its placement-stage trade-off, but CTS exposed a local congestion problem that remained concentrated near macros and `ex_stage_i`.

## Source

`docs/original-reports/03_floorplan_and_placement_iterations_2_to_8.docx`, pages 4–8.
