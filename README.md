# CVA6 RISC-V Physical Design — Post-Route Case Study

![EDA Flow](https://shields.io)
![Architecture](https://shields.io)
![Stage](https://shields.io)
![DRC Status](https://shields.io)

> **Project Scope Note:** This README documents the downstream physical-design stages and results explicitly captured in the supplied run analysis. It reflects the **post-route timing-fix stage** and does not claim GDSII generation, foundry signoff, tape-out, or full-chip timing closure.

---

## 📋 Project Summary

| Attribute | Details |
| :--- | :--- |
| **Design Target** | CVA6 single-clock RISC-V processor |
| **Implementation Focus** | Physical-design analysis, congestion evaluation, CTS, routing, DRC cleanup, and timing optimization |
| **Primary EDA Tool** | Cadence Innovus *(Cadence Genus used in the broader RTL-to-GDSII flow)* |
| **Current Milestone** | Post-route timing-fix stage |

### Overview
This case study documents the downstream physical-design work performed on the CVA6 implementation. Rather than presenting a single Quality of Results (QoR) snapshot, this repository highlights **engineering decisions and report-driven iteration** across placement evaluation, clock-tree synthesis (CTS), post-CTS analysis, routing, post-route DRC cleanup, and path-group timing fixes.

---

## 📊 Stage-by-Stage Progression

### 1. Placement Experiment & Database Selection
The selected placement baseline advanced to CTS was chosen based on congestion heatmaps and floorplan metrics:

* **Floorplan Dimensions:** `950 × 916` (`850,552 µm²` total area)
* **Macro-to-Macro Channel:** `20 µm`
* **Partial Placement Blockage:** `30%` density between macros
* **Placement Core Utilization:** `82.6%`
* **Global-Route Overflow:** `0.20%`
* **Hotspot Score:** `1.56`

---

### 2. Clock-Tree Synthesis (CTS) & Post-CTS Evaluation

| Stage | Core Utilization | Overflow (Total / H / V) | Hotspot Score | Key Engineering Findings |
| :--- | :--- | :--- | :--- | :--- |
| **CTS** | `83.2%` | `0.47%` (`0.11%` H, `0.36%` V) | `14.83` | Identified max-transition violations needing attention before downstream steps. |
| **Post-CTS** | `83.6%` | `0.53%` (`0.11%` H, `0.44%` V) | `16.39` | Reviewed reports for setup, hold, design-rule violations (DRVs), and congestion. |

---

### 3. Routing & Initial Post-Route Checks
Routing introduced congestion and physical rule violations that required targeted intervention:

* **Routing-Stage DRC Count:** `40`
* **Congestion Overflow:** `0.51%` (`0.20%` horizontal, `0.31%` vertical)
* **Hotspot Score:** `18.12`
* **Antenna Violations:** `0`
* **Initial Post-Route DRC Count:** `10` *(primarily via-related and wide-metal-jog [WMJ] issues)*

---

### 4. Post-Route DRC Cleanup
A two-step automated and manual cleanup strategy eliminated all reported post-route violations:

1. **Targeted Route ECO:** Reran routing with `route_via_with_block_pins_only true`.
2. **Manual Layout Intervention:** Cleared remaining wide-metal-jog (WMJ) issues by manually replacing and rotating vias over macro pins.
3. **Final Verification Result:** **`0` DRC violations** *(as recorded in the supplied run analysis; not a foundry signoff claim)*.

---

### 5. Timing Optimization & Path-Group Closure
Timing closure combined **LVT enablement**, **useful-skew analysis**, and **constraint adjustments tailored to specific path behaviors**:

| Path Group | Reported Status | Worst Negative Slack (WNS) | Engineering Fix Applied |
| :--- | :--- | :--- | :--- |
| **I2O** | Timing fixed | **`+0.028 ns`** | Inserted a `max_delay` constraint, as input-to-output paths lacked valid timing endpoints without an explicit bound. |
| **Default** | Timing fixed | **`+0.186 ns`** | Reduced I/O delay budget from `60%` to `40%` of the clock period for reset-port-to-register-`CDN`-pin recovery/removal checks. |

> **Timing Disclaimer:** These WNS results apply strictly to the `I2O` and `Default` path groups listed above and do not establish full-chip timing closure across all path groups or signoff corners.

---

## 💡 Key Engineering Takeaways

* **Continuous Congestion Tracking:** Congestion was tracked across placement, CTS, post-CTS, and routing using both overflow percentages and hotspot scores.
* **Clean Placement Baseline:** Starting CTS from a well-behaved placement (`0.20%` overflow, `1.56` hotspot score) established a solid foundation for downstream clock and signal routing.
* **Hybrid DRC Resolution:** Post-route DRC cleanup required pairing global router settings (`route_via_with_block_pins_only true`) with surgical, manual via rotation and replacement near macro pins.
* **Behavior-Driven Constraints:** Path groups were closed by matching timing constraints to actual circuit topology (explicit `max_delay` bounds for unconstrained I2O paths and adjusted I/O budgets for async reset recovery/removal paths).
* **Iterative Debugging Focus:** The workflow demonstrates systematic, report-driven physical design debugging rather than relying on a single final QoR snapshot.

---

## 📂 Source Evidence & Documentation

Detailed Innovus report snapshots, timing logs, and congestion heatmaps are archived in [`reports/CVA6_Run_Analysis_Source.docx`](reports/CVA6_Run_Analysis_Source.docx):

* **Pages 1–2:** Placement experiments and selected-database evidence
* **Pages 2–5:** CTS and post-CTS timing and congestion evidence
* **Pages 5–8:** Routing, post-route DRC, and congestion evidence
* **Pages 8–10:** LVT enablement and useful-skew timing experiments
* **Page 11:** Final path-group timing fixes

---

*Prepared from the CVA6 Run Analysis report supplied with this project. Internal working database filenames have been intentionally omitted from this public-facing case study.*
