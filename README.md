CVA6 RISC-V Physical Design — Post-Route Case Study
Project: CVA6 single-clock RISC-V processor
Implementation focus: Physical-design analysis, congestion evaluation, CTS, routing, DRC cleanup, and timing optimization
EDA flow: Cadence Innovus (physical implementation); Cadence Genus was used in the broader RTL-to-GDSII project flow
Status represented by this report: Post-route timing-fix stage; GDSII generation/signoff is not claimed here.

Overview
This case study documents the downstream physical-design work performed on the CVA6 implementation. The work progressed from placement evaluation through CTS, post-CTS analysis, routing, post-route DRC cleanup, and path-group timing fixes.

The emphasis is on the engineering decisions and report-driven iteration rather than presenting a single QoR snapshot.
Implementation and experiment log
1. Placement experiment and database selection
The selected placement experiment used:
- Floorplan dimensions: 950 × 916
- Floorplan area: 850,552 µm²
- Macro-to-macro channel: 20 µm
- Partial placement blockage between macros: 30% density
- Placement core utilization: 82.6%
- Global-route overflow: 0.20%
- Hotspot score: 1.56
The report's congestion heatmap and metrics supported advancing this placement result to CTS. This is the selected placement baseline documented in the run analysis.
2. Clock-tree synthesis and post-CTS evaluation
Stage	Core utilization	Overflow	Hotspot score
CTS	83.2%	0.47% (0.11% H, 0.36% V)	14.83
Post-CTS	83.6%	0.53% (0.11% H, 0.44% V)	16.39


The CTS assessment identified maximum-transition violations that needed attention before downstream implementation. The post-CTS reports were reviewed for setup, hold, design-rule violations, and congestion.
3. Routing and initial post-route checks
The routing report recorded:
- Routing-stage DRC count: 40
- Congestion overflow: 0.51% (0.20% horizontal, 0.31% vertical)
- Hotspot score: 18.12
- Antenna violations: 0
The initial post-route report showed 10 DRC violations. The violation summary included via-related and wide-metal-jog (WMJ) issues.
4. Post-route DRC cleanup
A route ECO was rerun with the route_via_with_block_pins_only true setting. The remaining WMJ issues were manually cleared by replacing and rotating vias over macro pins.

The report records the resulting DRC verification as:
- Final DRC violations: 0
This result refers to the DRC verification shown in the supplied run analysis; it should not be interpreted as a complete foundry signoff or tape-out claim.
5. Timing optimization and path-group closure
The run analysis documents three timing-related steps: LVT enablement, useful-skew analysis, and path-group-specific timing fixes.

The final timing-fix section records:
Path group	Reported status	WNS
I2O	Timing fixed	+0.028 ns
Default	Timing fixed	+0.186 ns


I2O path approach: A maximum-delay constraint was inserted. The report explains that input-to-output paths did not have valid timing endpoints for closure without an explicit maximum-delay constraint.

Default path approach: The I/O delay budget was reduced from 60% to 40% of the clock period. The report identifies these paths as reset-port-to-register-CDN-pin paths, for which recovery and removal checks apply.

These WNS values apply to the two path groups listed above. They do not establish that every timing group or all signoff checks are closed.
Key engineering observations
- Congestion was tracked across placement, CTS, post-CTS, and routing using both overflow and hotspot metrics.
- The placement result selected for CTS had 0.20% overflow and a 1.56 hotspot score.
- Routing introduced DRC work that required a targeted route ECO and manual via-level cleanup around macro pins.
- Timing closure was treated by path group, with constraints selected to match the path behavior described in the reports.
- The report captures iterative implementation and debugging, not merely a final screenshot.
Evidence
The accompanying source report contains the detailed Innovus report snapshots and congestion heatmaps:
- Placement and selected-database evidence: pages 1–2
- CTS and post-CTS timing/congestion evidence: pages 2–5
- Routing, post-route DRC, and congestion evidence: pages 5–8
- LVT and useful-skew timing experiments: pages 8–10
- Final path-group timing fixes: page 11
See [`reports/CVA6_Run_Analysis_Source.docx`](reports/CVA6_Run_Analysis_Source.docx).
Scope and status
This write-up describes the stages and results explicitly captured in the supplied run analysis. It does not claim GDSII output generation, foundry signoff, tape-out, or full-chip timing closure. Those should only be added when separately verified.
Prepared from the CVA6 Run Analysis report supplied with this project. Internal working database filenames have been intentionally omitted from this public-facing case study.
