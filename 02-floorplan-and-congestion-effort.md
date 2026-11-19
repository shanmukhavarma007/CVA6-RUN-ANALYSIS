# 02 – Floorplan Dimension and Congestion-Effort Exploration

## Floorplan change

The report describes changing the floorplan dimensions by:

- Width: **+16.8 µm**
- Height: **−16.8 µm**
- Updated dimensions shown as **950 × 916 µm** (width × height)

After macro placement, core utilization was reported as **72%**. The initial default run is separately reported at **83%** core utilization.

The report includes a timing summary and congestion/hotspot evidence for the initial run.

## Next-run settings

Because timing was described as “not so bad,” the next experiment increased:

- `place_global_cong_Effort` to `high`
- `place_Detail_wire_length_opt_Effort` to `high`

## Run 2 – High congestion effort

The report records **83.4%** core utilization and includes congestion and timing screenshots. It does not provide a clear textual verdict or a complete set of numeric timing values in the extracted text.

**Interpretation limited to source:** this run tests higher global congestion effort and detail wire-length optimization while retaining the floorplan context. The source screenshots should be consulted for detailed timing and congestion values.

## Source

`docs/original-reports/03_floorplan_and_placement_iterations_2_to_8.docx`, pages 1–4.
