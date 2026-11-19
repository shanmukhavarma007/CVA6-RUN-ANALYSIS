# 00 – Baseline Diagnosis and Cluster-Based Placement Proposal

## Observation

The initial analysis identified timing violations after placement. The report attributes the issue to the long channel between two macro groups and highlights:

- No major congestion or cell-density issue was observed.
- Post-placement core utilization was reported as **82.3%**.
- Register-to-register paths in the `ex_stage_i` hierarchy showed higher logic depth.
- Larger inverters and buffers were present on those register-to-register paths.

## Proposed change

The report suggested bounding `ex_stage_i` cells together so that their registers would be placed closer, reducing inter-register route length.

The next decision was to proceed with a **cluster-based placement bound** intended to keep `ex_stage_i` cells physically close.

## Predictions recorded before the run

- Timing could improve because many register-to-register paths belonged to `ex_stage_i`.
- Core utilization might reduce if fewer buffers were needed.
- Global cell density might remain similar.
- Slight congestion near macro boundaries was possible because `ex_stage_i` cells communicate with macros.

## Reported follow-up

The source includes default-run timing evidence, a congestion image, and a post-CTS utilization value of **82.44%**. It does not provide a complete before/after numeric comparison for the cluster-bound experiment in the extracted text.

**Interpretation:** This is a hypothesis-driven placement experiment: the physical clustering decision was based on the identified `ex_stage_i` path structure, with macro-boundary congestion explicitly recognized as a possible trade-off.

## Source

`docs/original-reports/01_cluster_based_placement_analysis.docx` (5 pages; see the placement/congestion images and post-CTS report).
