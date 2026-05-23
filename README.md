# nasscom VSD SoC Design Program - Physical Design Track

## Day-by-Day Physical Design Guide - OpenLANE + Sky130 PDK

This repository documents the complete **physical design journey** of the VSD SoC Design Program, covering the full RTL-to-GDSII flow using **OpenLANE** automated flow with the **Sky130 Process Design Kit (PDK)**.

---

## Program Overview

| Module | Description |
|--------|-------------|
| **Day 1** | Docker setup, OpenLANE launch, and logic synthesis of picorv32a |
| **Day 2** | Floorplan and placement - die area planning and cell placement |
| **Day 3** | Standard cell library design - CMOS inverter with ngspice characterization |
| **Day 4** | Clock Tree Synthesis (CTS) and timing analysis with OpenSTA |

---

## Day 1: Environment Setup and Synthesis

### Docker Environment Setup

OpenLANE runs inside Docker for reproducibility:

```bash
cd openlane
./flow.tcl -interactive

# Inside OpenLANE prompt
% prep -design picorv32a
% run_synthesis
```

### Logic Synthesis of picorv32a

**Flop Ratio:** ~0.1084 (10.84% DFFs)
- Methodology: DFF count / total cell count in synthesized netlist
- Indicates sequential vs combinational balance

---

## Day 2: Floorplan and Placement

### Floorplan

```bash
% run_floorplan
```

Defines core area, die size, PDN, I/O placement.
- Distance units in OpenLANE are in microns
- Area utilization target: 60-80%

### Placement

```bash
% run_placement
```

Places standard cells within floorplan. Visualized with Magic:

```bash
magic -T sky130A.tech [layout_file]
```

---

## Day 3: Standard Cell Library Design

### CMOS Inverter Design

```bash
# Clone cell design repo
git clone https://github.com/efabless/vsdstdcelldesign

# Extract layout to SPICE
magic -T sky130A.tech -dcols 0 invgrundraw.i lef -met1
ext2spice invgrundraw.i

# Run SPICE simulation
ngspice inv.spice
```

### Characterization

| Parameter | Description |
|-----------|-------------|
| **Rise Time** | 10% to 90% output transition |
| **Fall Time** | 90% to 10% output transition |
| **Propagation Delay** | Input to 50% output transition |
| **Power** | Static and dynamic dissipation |

---

## Day 4: Clock Tree Synthesis and Timing Analysis

### Clock Tree Synthesis

```bash
% run_cts
```

TritonCTS builds clock distribution to minimize skew.

### Timing Analysis with OpenSTA

```bash
% report_checks -path_delay min_max
% report_tns
% report_wns
```

**Pre-CTS**: Ideal clock assumption
**Post-CTS**: Real clock network, verify timing closure

---

## Key Takeaways

- Completed RTL-to-GDSII flow for picorv32a RISC-V processor
- Designed and characterized standard cells at transistor level
- Performed CTS and static timing analysis
- Used Docker-based open-source EDA tools (OpenLANE, Sky130)
- Documented every stage with screenshots and reports

---

## Toolchain

| Tool | Purpose |
|------|--------|
| **OpenLANE** | Automated RTL-to-GDSII flow |
| **Sky130 PDK** | 130nm open-source PDK |
| **Magic** | Layout editor and DRC |
| **ngspice** | SPICE simulator |
| **OpenSTA** | Static timing analysis |
| **Docker** | Containerized execution |

---

## Author

**Krish Bhavsar**  
Electronics & Communication Engineering  
[GitHub: @Kribh19](https://github.com/Kribh19)  
bhavsar.krish33@gmail.com
