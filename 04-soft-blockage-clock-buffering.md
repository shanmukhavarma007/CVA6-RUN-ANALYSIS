# 04 – Soft Blockage and Clock-Path Buffering (Run 4)

## Strategy

The run-4 strategy combined two changes:

1. Replace the partial blockage between the two macro-hierarchy channels with a **soft blockage**, using a **15 µm channel gap**.
2. Increase clock-path drive strength by using `eco_add_repeater` for clock-net buffering.

The run retained high congestion effort and high wire-length effort. The stated prediction was to reduce overflow while maintaining handoff timing.

## CKBD20 insertion procedure recorded in the report

The source documents the following sequence for inserting CKBD20*LVT cells in the common clock path:

1. Remove the `dont_use` attribute/constraint from `CKBD20*LVT`.
2. Set `eco_honor_fixed_wires` to `false`.
3. Enable cell padding for the high-drive-strength CKBD20*LVT cell.
4. Use uniform padding on all sides, starting at 4 sites. The report notes a 4-site width of `4 × 0.14` for the 28 nm LEF and explains the decap-cell consideration.
5. Insert a guide buffer with `eco_add_repeater`, specifying the cell, instance name, location, and clock-port net.
6. Set `eco_honor_fixed_wires` back to `true`.
7. Insert buffers between the clock port and guide buffer using `eco_add_repeater` with `-spread_distance 200` and a spread prefix. The report notes that `spread_distance` should be inclusive when using the net switch.
8. Delete the temporary CKBD20*LVT cell padding.
9. Keep `dont_touch` on inserted cells and their respective nets.
10. Continue the placement flow.

## Result and decision

The source includes congestion and timing screenshots for this experiment. Its explicit decision is to move forward with the **run-3 database**, which it describes as having optimal timing and one of the best congestion-overflow results among the runs so far.

The subsequent CTS assessment on run 3 identifies macro-adjacent congestion, especially around `ex_stage_i` (documented on the run-3 page).

## Source

`docs/original-reports/03_floorplan_and_placement_iterations_2_to_8.docx`, pages 5–8.
