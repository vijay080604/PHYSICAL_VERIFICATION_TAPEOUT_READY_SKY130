# Physical Verification for Tapeout Readiness using SKY130

<p align="center">
    <img src="images/module_01/course_banner.png" alt="Physical Verification Workflow" width="900">
</p>

<p align="center">
Hands-on documentation of physical verification using the SKY130 Open PDK, covering DRC, LVS, layout extraction, verification debugging, and an overview of the OpenLANE RTL-to-GDSII flow.
</p>

---

## Repository Overview

This repository documents my hands-on learning from the **Physical Verification for Tapeout Readiness using SKY130** workshop conducted by **VLSI System Design (VSD)**.

It covers the practical physical verification flow using the **SKY130 Open PDK**, including **DRC, LVS, layout extraction, GDS handling, verification debugging**, and an overview of the **OpenLANE RTL-to-GDSII** flow through hands-on laboratory exercises.

The repository serves as a structured reference for understanding tapeout-ready physical verification using industry-relevant open-source EDA tools.

---
## Course Contributors

This workshop was conducted by **VLSI System Design (VSD)**. Special thanks to the following contributors for sharing their expertise and supporting the open-source VLSI community.

- **Kunal Ghosh** – [LinkedIn](https://www.linkedin.com/in/kunal-ghosh-vlsisystemdesign-com-28084836/)
- **R. Timothy Edwards** – [GitHub](https://github.com/RTimothyEdwards)
## Tools & Technologies

The following open-source tools and technologies were used throughout this workshop.

| Category | Tools |
|:---------|:------|
| **Process Design Kit (PDK)** | SKY130 Open PDK |
| **Design & Verification** | Magic, Xschem, Netgen, KLayout |
| **Simulation** | ngspice |
| **RTL-to-GDSII Flow** | OpenLANE, OpenROAD |
## Repository Navigation

| Module | Topics Covered |
|:-------|:---------------|
| **[Module 01 – Environment Setup & Physical Verification Flow](#module-01--environment-setup--physical-verification-flow)** | Tool setup, SKY130 PDK and physical verification workflow |
| **[Module 02 – DRC & LVS Fundamentals](#module-02--drc--lvs-fundamentals)** | Design Rule Check (DRC) and Layout Versus Schematic (LVS) fundamentals |
| **[Module 03 – DRC Rule Analysis & Debugging](#module-03--drc-rule-analysis--debugging)** | DRC rule analysis, violations and debugging |
| **[Module 04 – Layout Versus Schematic (LVS)](#module-04--layout-versus-schematic-lvs)** | Layout extraction, LVS execution and debugging |
| **[OpenLANE RTL-to-GDSII Flow Overview](#openlane-rtl-to-gdsii-flow-overview)** | RTL-to-GDSII physical design flow overview |
---

## Module 01 – Environment Setup & Physical Verification Flow

This module covers the installation of the open-source EDA tools required for physical verification using the **SKY130 Open PDK**. These tools are used throughout the workshop for layout design, circuit simulation, layout extraction, DRC, and LVS verification.

---
<p align="center">
    <img src="images/module_01/physical_verification_flow.png" alt="Physical Verification Workflow" width="900">
</p>

<p align="center">
    <em>Figure 1. Physical verification workflow using the SKY130 Open PDK and open-source EDA tools.</em>
</p>

### Magic

**Magic** is an open-source VLSI layout editor used for layout design, extraction, and Design Rule Checking (DRC).

**Installation**

```bash
git clone git://opencircuitdesign.com/magic
cd magic
./configure
make
sudo make install
```

**Reference:** [Magic Documentation](http://opencircuitdesign.com/magic/index.html)

---

### Netgen

**Netgen** is an open-source Layout Versus Schematic (LVS) tool used to compare schematic and extracted layout netlists.

**Installation**

```bash
git clone git://opencircuitdesign.com/netgen
cd netgen
./configure
make
sudo make install
```

**Reference:** [Netgen Documentation](http://opencircuitdesign.com/netgen/index.html)

---

### Xschem

**Xschem** is an open-source schematic capture tool used to create transistor-level circuit schematics.

**Installation**

```bash
git clone https://github.com/StefanSchippers/xschem.git xschem_git
cd xschem_git
./configure
make
sudo make install
```

**Reference:** [Xschem Documentation](http://repo.hu/projects/xschem/index.html)

---

### ngspice

**ngspice** is an open-source SPICE simulator used for analog and mixed-signal circuit simulation.

**Installation**

```bash
tar -zxvf ngspice-37.tar.gz
cd ngspice-37
mkdir release
cd release
../configure --with-x --with-readline=yes --disable-debug
make
sudo make install
```

To display simulation waveforms:

```bash
sudo apt-get update
sudo apt-get install xterm
```

**Reference:** [ngspice Documentation](https://ngspice.sourceforge.io/index.html)

---

### Open_PDKs

**Open_PDKs** configures the complete **SKY130 Open PDK** environment required by tools such as Magic, Netgen, KLayout, and OpenLANE.

**Installation**

```bash
git clone git://opencircuitdesign.com/open_pdks
cd open_pdks
./configure --enable-sky130-pdk
make
sudo make install
```

**Reference:** [Open_PDKs GitHub Repository](https://github.com/RTimothyEdwards/open_pdks)

---

### Common Installation Commands

The following commands are frequently used while building open-source EDA tools from source.

| Command | Description |
|:--------|:------------|
| `git clone` | Downloads the project source code from its repository. |
| `cd <directory>` | Navigates to the project directory. |
| `./configure` | Configures the build environment and checks required dependencies. |
| `make` | Compiles the source code into executable binaries. |
| `sudo make install` | Installs the compiled application on the system. |
| `tar -zxvf` | Extracts compressed source packages. |
| `mkdir` | Creates a new directory (commonly used for out-of-source builds). |

---

With the required tools successfully installed, the physical verification environment is ready. The next step is to understand the complete physical verification design flow before moving on to DRC and LVS verification.

<p align="right">
    <a href="#repository-navigation">Back to Navigation ↑</a>
</p>

### SKY130 PDK Integration

Before starting the laboratory exercises, verify that the **SKY130 Open PDK** is installed correctly and configure the required technology files for **Xschem**, **Magic**, and **Netgen**.

#### 1. Verify SKY130 PDK Installation

Use the following commands to confirm that the SKY130 PDK is available on your system.

```bash
ls /usr/share/pdk
ls /usr/share/pdk/sky130A
```

**Purpose:** Verifies that the SKY130 Open PDK is installed and accessible before configuring the design environment.

---

#### 2. Create the Project Workspace

Create a working directory and organize separate folders for each tool.

```bash
mkdir inverter
cd inverter

mkdir mag
mkdir netgen
mkdir xschem
```

**Purpose:** Creates a structured workspace for Magic, Netgen, and Xschem configuration files.

---

#### 3. Configure the SKY130 PDK

Link the required SKY130 technology files to each tool using symbolic links.

```bash
cd xschem

ln -s /usr/share/pdk/sky130A/libs.tech/xschem/xschemrc
ln -s /usr/share/pdk/sky130A/libs.tech/ngspice/spinit .spiceinit

cd ../mag

ln -s /usr/share/pdk/sky130A/libs.tech/magic/sky130A.magicrc .magicrc

cd ../netgen

ln -s /usr/share/pdk/sky130A/libs.tech/netgen/sky130A_setup.tcl setup.tcl
```

**Purpose:** Links the required SKY130 configuration files, enabling the EDA tools to use the correct technology libraries during layout, simulation, and LVS.

---

> **Note**
>
> The SKY130 PDK installation path may vary depending on your environment.
>
> - **VSD Cloud Workspace (used in this workshop):**
>   ```
>   /usr/share/pdk/sky130A/
>   ```
>
> - **Some Linux installations:**
>   ```
>   /usr/local/share/pdk/sky130A/
>   ```
>
> Verify the installation path before creating the symbolic links.
<p align="right">
    <a href="#repository-navigation">Back to Navigation ↑</a>
</p>
#### Integration Verification

The following screenshots confirm that each tool is successfully configured and linked with the **SKY130 Open PDK** environment.

<table align="center">
<tr>
<td align="center">
<img src="images/module_01/xschem_working.png" width="420"><br>
<em>Figure 2. Xschem successfully configured with SKY130 PDK.</em>
</td>

<td align="center">
<img src="images/module_01/ngspice_working.png" width="420"><br>
<em>Figure 3. ngspice successfully configured with SKY130 PDK.</em>
</td>
</tr>

<tr>
<td align="center">
<img src="images/module_01/magic_working.png" width="420"><br>
<em>Figure 4. Magic successfully loaded with the SKY130 technology file.</em>
</td>

<td align="center">
<img src="images/module_01/netgen_working.png" width="420"><br>
<em>Figure 5. Netgen successfully configured for LVS using SKY130.</em>
</td>
</tr>
</table>
---

## CMOS Inverter Design & Verification Workflow

This example demonstrates the complete physical verification workflow using a CMOS inverter. Starting from schematic design, the circuit is simulated, implemented as a physical layout, extracted into a SPICE netlist, and finally verified using LVS with Netgen.

---

### Step 1 – CMOS Inverter Schematic

The CMOS inverter schematic is created in **Xschem** using the SKY130 device library. After placing the PMOS and NMOS devices, the required device parameters are configured before generating the circuit symbol for simulation.

<p align="center">
    <img src="images/module_01/inverter_schematic.png" width="900">
</p>

<p align="center">
    <em>Figure 6. CMOS inverter schematic created in Xschem using the SKY130 device library.</em>
</p>

---

### Step 2 – Circuit Simulation

A dedicated testbench is created using the generated inverter symbol to verify the circuit operation in **ngspice**. The voltage transfer characteristics (VTC) confirm the expected switching behaviour of the CMOS inverter.

<p align="center">
    <img src="images/module_01/inverter_tb_schematic.png" width="900">
</p>

<p align="center">
    <em>Figure 7. Testbench schematic used for CMOS inverter simulation.</em>
</p>

<p align="center">
    <img src="images/module_01/ngspice_with_vtc_charateristics.png" width="900">
</p>

<p align="center">
    <em>Figure 8. ngspice simulation showing the generated VTC characteristics.</em>
</p>

<p align="center">
    <img src="images/module_01/cmos_inverter_vtc_output.png" width="700">
</p>

<p align="center">
    <em>Figure 9. CMOS inverter Voltage Transfer Characteristic (VTC).</em>
</p>

> **Observation:** The simulation confirms the expected inverter switching behaviour before proceeding to physical layout implementation.

---

### Step 3 – Physical Layout Implementation

The verified schematic is implemented as a physical layout in **Magic** using the SKY130 technology. Device placement and routing are completed before extracting the layout netlist.

<p align="center">
    <img src="images/module_01/layout1_cmos_inverter.png" width="900">
</p>

<p align="center">
    <em>Figure 10. CMOS inverter layout implemented using Magic.</em>
</p>

---

### Step 4 – Layout Extraction

After completing the layout, the extracted SPICE netlist is generated using the following Magic commands.

```tcl
extract do local
extract all
ext2spice lvs
ext2spice
```

| Command | Purpose |
|:--------|:--------|
| `extract do local` | Enables local layout extraction. |
| `extract all` | Extracts the complete layout. |
| `ext2spice lvs` | Generates an LVS-compatible SPICE netlist. |
| `ext2spice` | Generates the extracted SPICE netlist from the layout. |

<p align="center">
    <img src="images/module_01/extract_file_proof.png" width="900">
</p>

<p align="center">
    <em>Figure 11. Layout extraction completed successfully.</em>
</p>

<p align="center">
    <img src="images/module_01/spice_extraction.png" width="900">
</p>

<p align="center">
    <em>Figure 12. Extracted SPICE netlist generated from the Magic layout.</em>
</p>

---

### Step 5 – Layout Versus Schematic (LVS)

The extracted layout netlist is compared with the original schematic netlist using **Netgen**.

```bash
netgen -batch lvs "../mag/inverter.spice inverter" "../xschem/inverter.spice inverter"
```

**Purpose:** Performs Layout Versus Schematic (LVS) verification by comparing the extracted layout netlist with the schematic netlist.

<p align="center">
    <img src="images/module_01/lvs_command_proof1.png" width="900">
</p>

<p align="center">
    <em>Figure 13. Executing LVS verification using Netgen.</em>
</p>

<p align="center">
    <img src="images/module_01/lvs_result.png" width="900">
</p>

<p align="center">
    <em>Figure 14. LVS comparison result generated by Netgen.</em>
</p>

> **Observation:** The LVS comparison reports mismatches between the schematic and layout netlists. This example is included to demonstrate the complete verification workflow and LVS execution process rather than achieving a fully matched LVS result.

<p align="right">
    <a href="#repository-navigation">Back to Navigation ↑</a>
</p>
---

## Module 02 – DRC & LVS Fundamentals

This module introduces the fundamental concepts of **Design Rule Checking (DRC)** and **Layout Versus Schematic (LVS)** using the **SKY130 Open PDK**. It demonstrates how layouts are imported, extracted, verified against foundry design rules, and compared with the original schematic to ensure design correctness.

### DRC & LVS Verification Flow

<p align="center">
    <img src="images/module_02/drc_lvs_fundamentals_flow.png" alt="DRC and LVS Fundamentals Flow" width="100%">
</p>

<p align="center">
    <em>Figure 15. Overview of the physical verification flow from schematic design to layout verification using DRC and LVS.</em>
</p>

---
### Reading Standard Cell Layouts

Before loading the standard-cell GDS, copy the **SKY130 Magic technology file** into the working directory to ensure the layout is opened with the correct SKY130 process configuration.

#### Commands Used

```tcl
cp /usr/share/pdk/sky130A/libs.tech/magic/sky130A.magicrc ./.magicrc

gds read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/gds/sky130_fd_sc_hd.gds

load sky130_fd_sc_hd__and2_1
```

<p align="center">
    <img src="images/module_02/gds_read_01.png" width="900">
</p>

<p align="center">
    <em>Figure 16. Copying the SKY130 Magic technology file and importing the standard-cell GDS library.</em>
</p>

<p align="center">
    <img src="images/module_02/reading_gds_cell_02.png" width="900">
</p>

<p align="center">
    <em>Figure 17. Reading the SKY130 standard-cell GDS database into Magic.</em>
</p>

<p align="center">
    <img src="images/module_02/selection_and_2_cell_03.png" width="900">
</p>

<p align="center">
    <em>Figure 18. Selecting and loading the <code>sky130_fd_sc_hd__and2_1</code> standard cell.</em>
</p>

<p align="center">
    <img src="images/module_02/and_2_1_cell_layout_view_04.png" width="900">
</p>

<p align="center">
    <em>Figure 19. Layout view of the <code>sky130_fd_sc_hd__and2_1</code> standard cell in Magic.</em>
</p>

> **Observation:** The SKY130 AND2 standard cell is successfully imported and loaded into Magic, making it ready for further physical verification operations such as metadata configuration, extraction, DRC, and LVS.
> ---

### Configuring Ports & Layout Metadata

To enable accurate physical verification, the standard cell layout is configured with the required **port information** and **metadata**. This ensures proper mapping between the layout and the corresponding SPICE netlist during LVS verification.

#### Commands Used

```tcl
port first
port 1 name
port 1 class
port 1 use

lef read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/lef/sky130_fd_sc_hd.lef

readspice /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/spice/sky130_fd_sc_hd.spice

load sky130_fd_sc_hd__and2_1
```
<p align="center">
    <img src="images/module_02/05_port_configuration.png" width="900">
</p>

<p align="center">
    <em>Figure 20. Configuring port information for the standard cell.</em>
</p>

<p align="center">
    <img src="images/module_02/06_lef_import.png" width="900">
</p>

<p align="center">
    <em>Figure 21. Importing LEF data for the SKY130 standard-cell library.</em>
</p>
<p align="center">
    <img src="images/module_02/read_spice_library_07.png" width="900">
</p>

<p align="center">
    <em>Figure 22. SPICE library loaded successfully, providing the netlist information required for LVS verification.</em>
</p>
---

### Working with Standard Cell Instances

Magic supports hierarchical layout design by allowing existing standard cells to be loaded, instantiated, and edited. This section demonstrates the difference between **`load`** and **`getcell`**, along with the process of making an imported cell writable for modification.

#### Commands Used

```tcl
lef read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/lef/sky130_fd_sc_hd.lef

load sky130_fd_sc_hd__and2_1

readspice /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/spice/sky130_fd_sc_hd.spice

load test

getcell sky130_fd_sc_hd__and2_1

gds write test

property

cellname writeable sky130_fd_sc_hd__and2_1 true

gds write test
```

> **Note**
>
> - **`load`** opens an existing layout cell for viewing or editing.
> - **`getcell`** places an instance of an existing cell inside the current layout, enabling hierarchical design.

<p align="center">
    <img src="images/module_02/get_cell_08.png" width="900">
</p>

<p align="center">
    <em>Figure 23. Placing the <code>sky130_fd_sc_hd__and2_1</code> standard cell as an instance using <code>getcell</code>.</em>
</p>

<p align="center">
    <img src="images/module_02/gds_write_09.png" width="900">
</p>

<p align="center">
    <em>Figure 24. Making the imported standard cell writable before exporting the updated layout.</em>
</p>

> **Observation:** The standard cell is successfully instantiated and marked as writable, allowing hierarchical layouts to be modified and saved for further physical verification.
> ---

### Layout Extraction

Once the layout is completed, **Magic** extracts the electrical connectivity and generates a SPICE netlist from the physical layout. The extracted netlist serves as the input for subsequent LVS verification.

#### Commands Used

```tcl
extract all

ext2spice lvs

ext2spice
```

<p align="center">
    <img src="images/module_02/layout_extraction_10.png" width="900">
</p>

<p align="center">
    <em>Figure 25. Extracting the layout and generating an LVS-compatible SPICE netlist.</em>
</p>

<p align="center">
    <img src="images/module_02/extracted_spice_netlist_11.png" width="900">
</p>

<p align="center">
    <em>Figure 26. SPICE netlist generated from the extracted layout.</em>
</p>

> **Observation:** The layout is successfully extracted into a SPICE netlist, providing the electrical representation required for LVS verification.
> #### Advanced Extraction Options

Magic also provides additional extraction commands for generating simulation files and including parasitic resistance and capacitance information.

##### Commands Used

```tcl
ext2spice cthresh 0.01
ext2spice

ext2sim labels on
ext2sim

extresist tolerance 10
extresist

ext2spice lvs
ext2spice cthresh 0.01
ext2spice extresist on
ext2spice
```
<p align="center">
    <img src="images/module_02/spice_capacitance_extraction_12.png" width="900">
</p>

<p align="center">
    <em>Figure 27. Generating the extracted SPICE netlist with capacitance information.</em>
</p>
<p align="center">
    <img src="images/module_02/extracted_spice_netlist_of_cthresh_13.png" width="900">
</p>

<p align="center">
    <em>Figure 28. SPICE netlist generated from the extracted layout.</em>
</p>
<p align="center">
    <img src="images/module_02/spice_resistance_extraction_14.png" width="900">
</p>

<p align="center">
    <em>Figure 29. Generating the extracted SPICE netlist with resistance information.</em>
</p>
</p>
<p align="center">
    <img src="images/module_02/extraction_files_15.png" width="900">
</p>
<p align="center">
    <em>Figure 30.files created after all extraction operations</em>
</p>

---

### Design Rule Check (DRC)

Design Rule Check (DRC) verifies that the layout complies with the manufacturing rules defined by the **SKY130 Open PDK**. It identifies spacing, width, enclosure, and connectivity violations that could affect fabrication.

#### Commands Used

```bash
/usr/share/pdk/sky130A/libs.tech/magic/run_standard_drc.py \
/usr/share/pdk \
sky130A/libs.ref/sky130_fd_sc_hd/mag/sky130_fd_sc_hd__and2_1.mag
```

```tcl
drc listall why

drc style drc(full)

drc check

drc why

drc find

getcell sky130_fd_sc_hd__tapvpwrvgnd_1
```

<p align="center">
    <img src="images/module_02/drc_execution_17.png" width="900">
</p>

<p align="center">
    <em>Figure 31. Running Design Rule Check (DRC) on the SKY130 standard cell.</em>
</p>

<p align="center">
    <img src="images/module_02/drc_violation_16.png" width="900">
</p>

<p align="center">
    <em>Figure 32. Viewing DRC results and locating rule violations in Magic.</em>
</p>

> **Observation:** The DRC engine successfully checks the layout against the SKY130 design rules and reports any rule violations that require correction before fabrication.
> After confirming that the layout satisfies the required design rules, the next step is to verify its electrical equivalence with the schematic using **Layout Versus Schematic (LVS)**.
> ---

### Layout Versus Schematic (LVS)

After the layout successfully passes DRC, the next step is to verify its electrical equivalence with the original schematic. **Netgen** compares the extracted layout netlist against the reference SPICE netlist and reports whether both designs match.

#### Commands Used

```bash
netgen -batch lvs \
"../mag/sky130_fd_sc_hd__and2_1.spice sky130_fd_sc_hd__and2_1" \
"/usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/spice/sky130_fd_sc_hd.spice sky130_fd_sc_hd__and2_1"
```

<p align="center">
    <img src="images/module_02/lvs_execution_18.png" width="900">
</p>

<p align="center">
    <em>Figure 33. Executing Layout Versus Schematic (LVS) verification using Netgen.</em>
</p>

<p align="center">
    <img src="images/module_02/lvs_result_19.png" width="900">
</p>

<p align="center">
    <em>Figure 34. LVS comparison between the extracted layout netlist and the reference SPICE netlist.</em>
</p>

> **Observation:** Netgen successfully compares the extracted layout with the reference schematic netlist and reports the LVS verification result.

After confirming the electrical equivalence between the layout and schematic, the final verification step is to compare layouts using **XOR verification**.
---

### XOR Verification

XOR verification compares two layout databases and highlights any geometric differences between them. This technique is commonly used to validate layout modifications and identify unintended changes.

#### Commands Used

```tcl
flatten -nolabels xor_test

xor -nolabels xor_test

load xor_test
```

<p align="center">
    <img src="images/module_02/xor_verification_20.png" width="900">
</p>

<p align="center">
    <em>Figure 35. Performing XOR verification to compare layout differences.</em>
</p>

> **Observation:** XOR verification highlights any geometric differences between the reference and modified layouts, helping identify unintended layout changes.


## Module 02 – Physical Verification Command Reference

| Command | Purpose |
|:--------|:--------|
| `gds read` | Imports a GDS layout into Magic. |
| `load` | Opens an existing layout cell for viewing or editing. |
| `getcell` | Places an instance of a cell into the current layout. |
| `lef read` | Imports LEF information for standard cells. |
| `readspice` | Loads the reference SPICE netlist into Magic. |
| `extract all` | Extracts the complete layout geometry. |
| `ext2spice lvs` | Generates an LVS-compatible SPICE netlist. |
| `ext2spice` | Generates the extracted SPICE netlist. |
| `ext2spice cthresh 0.01` | Includes parasitic capacitance above the specified threshold. |
| `ext2sim labels on` | Preserves layout labels in the simulation netlist. |
| `ext2sim` | Generates a simulation netlist from the extracted layout. |
| `extresist tolerance 10` | Sets the tolerance for resistance extraction. |
| `extresist` | Extracts parasitic resistance from the layout. |
| `ext2spice extresist on` | Includes extracted resistance in the SPICE netlist. |
| `drc style drc(full)` | Enables the complete SKY130 DRC rule deck. |
| `drc check` | Runs Design Rule Check on the current layout. |
| `drc why` | Displays the reason for a DRC violation. |
| `drc find` | Navigates to the detected DRC violation. |
| `netgen -batch lvs` | Performs Layout Versus Schematic verification. |
| `flatten -nolabels` | Creates a flattened layout for comparison. |
| `xor -nolabels` | Compares two layouts and highlights geometric differences. |
<p align="right">
    <a href="#repository-navigation">Back to Navigation ↑</a>
</p>
---

---

# Module 03 – DRC Rule Analysis & Debugging

This module focuses on understanding, analyzing, and debugging **Design Rule Check (DRC)** violations using the **SKY130 Open PDK**. It introduces common layout rule violations, explains why they occur, and demonstrates practical techniques for identifying and fixing them using **Magic Layout** before achieving a DRC-clean design.

## DRC Rule Analysis & Debugging Flow

<p align="center">
    <img src="images/module_03/drc_rule_analysis_debugging_flow.png" alt="DRC Rule Analysis and Debugging Flow" width="100%">
</p>

<p align="center">
    <em>Figure 36. Overview of the DRC rule analysis and debugging workflow followed throughout this module.</em>
</p>

---
### Reference and Resources

This module follows the design rules defined by the official **SKY130 Open PDK** documentation. The practical demonstrations cover only a subset of the complete rule deck used during physical verification.

- **Official SKY130 Design Rules:** https://skywater-pdk.readthedocs.io/en/main/rules.html
- **SKY130 PDK Documentation:** https://skywater-pdk.readthedocs.io/en/main/
## Magic Layout Shortcut Keys

| Shortcut | Function |
|:---------|:---------|
| **Left Click** | Change the layout box (cursor) position. |
| **Right Click** | Draw the selection box. |
| **Middle Click** | Paint the selected layer inside the box. |
| **G** | Toggle layout grid. |
| **U** | Undo the previous action. |
| **Shift + U** | Redo the last undone action. |
| **I** | Select the top-most cell. |
| **F** | Mirror the selected object. |
| **Z** | Zoom in. |
| **Shift + Z** | Zoom out. |
| **A** | Select everything inside the box. |
| **Shift + A** | Add to the current selection. |
| **C** | Copy the selected object. |
| **E** | Edit the selected cell. |
| **O** | Open the selected cell. |
| **4 / 8 / 6 / 2** | Move the selected object left, up, right, and down. |

## Frequently Used Magic Commands

| Command | Purpose |
|:--------|:--------|
| `drc find` | Locate the next DRC violation. |
| `label <name>` | Create a label for a layout terminal. |
| `extract all` | Extract the complete layout. |
| `ext2spice` | Generate the extracted SPICE netlist. |
| `getcell <file.mag>` | Insert an existing cell into the current layout. |
| `load <file.mag>` | Open a layout for editing. |
| `x` | Expand and view the contents of the selected cell. |
## VLSI Terminology

| Term | Description |
|:-----|:------------|
| **DRC** | Design Rule Check used to verify layout manufacturability. |
| **PDK** | Process Design Kit containing technology files, devices, and design rules. |
| **Tapeout** | Final GDSII layout submission for fabrication. |
| **Foundry** | Semiconductor fabrication facility where chips are manufactured. |
| **Technology Node** | Process feature size used during fabrication (e.g., SKY130). |
| **GDSII** | Standard file format used to exchange IC layout data. |
## Density Fill Generation

The following commands are used to generate and load metal fill patterns for density verification.

### Commands Used

```bash
/usr/share/pdk/sky130A/libs.tech/magic/check_density.py exercise_11.gds

/usr/share/pdk/sky130A/libs.tech/magic/generate_fill.py exercise_11.mag
```

```tcl
load exercise_11

box values 0 0 0 0

getcell exercise_11_fill_pattern child 0 0
```

> **Purpose:** Generates and loads metal fill patterns required to satisfy density rules before tapeout.
> > 📄 **Quick Reference:** [Magic Layout Shortcut Keys & Commands (PDF)](images/module_03/Magic_rules_and_short_keys.pdf)
> ---
>
> ---

## Practical 01 – Minimum Width Rule

### Objective

Understand how the minimum width rule is checked and why narrow geometries violate the SKY130 design rules.

#### Commands Used

```tcl
drc check

drc why

drc find
```
<table align="center">
<tr>
<td align="center" width="50%">

<img src="images/module_03/minimum_width_rule_01a_before.png" width="100%">

**Figure 37.** Minimum width rule violation (**Before Fix**).

</td>

<td align="center" width="50%">

<img src="images/module_03/minimum_width_rule_01a_after.png" width="100%">

**Figure 38.** Minimum width rule after applying the required fix.

</td>
</tr>
</table>

<br>

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/minimum_width_rule_01b_before.png" height="250">

**Figure 39.** Minimum spacing rule violation (**Before Fix**).

</td>

<td align="center" width="50%">

<img src="images/module_03/minimum_width_rule_01b_after.png" height="250">

**Figure 40.** Minimum spacing rule after applying the required fix.

</td>

</tr>
</table>
<br>

<table align="center">
<tr>
<td align="center" width="50%">

<img src="images/module_03/minimum_width_rule_01c_before.png" width="100%">

**Figure 41.** Wide spacing rule violation (**Before Fix**).

</td>

<td align="center" width="50%">

<img src="images/module_03/minimum_width_rule_01c_after.png" width="100%">

**Figure 42.** Wide spacing rule after applying the required fix.

</td>
</tr>
</table>

<br>

<table align="center">
<tr>
<td align="center" width="50%">

<img src="images/module_03/minimum_width_rule_01d_before.png" width="100%">

**Figure 43.** Notch rule violation (**Before Fix**).

</td>

<td align="center" width="50%">

<img src="images/module_03/minimum_width_rule_01d_after.png" width="100%">

**Figure 44.** Notch rule after applying the required fix.

</td>
</tr>
</table>




## Practical 02 – Via Rules

### Objective

Understand the fundamental **via-related design rules** in the SKY130 PDK, including via dimensions, multiple via placement, overlap requirements, and automatic via generation. These checks ensure reliable electrical connectivity between adjacent metal layers.

#### Commands Used

```tcl
drc check
drc why
drc find
```

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/via_size_rule_2a.png" width="100%">

**Figure 45.** Via size rule verification.

</td>

<td align="center" width="50%">

<img src="images/module_03/multiple_via_placement_2b.png" width="100%">

**Figure 46.** Multiple via placement verification.

</td>

</tr>
</table>

<br>

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/via_overlap_rule_2c.png" width="100%">

**Figure 47.** Via overlap rule verification.

</td>

<td align="center" width="50%">

<img src="images/module_03/automatic_via_generation_2d.png" width="100%">

**Figure 48.** Automatic via generation using Magic.

</td>

</tr>
</table>

---

## Practical 03 – Area Rules

### Objective

Understand the **minimum area** and **minimum hole** rules defined in the SKY130 PDK. These rules ensure that layout geometries meet the minimum manufacturable area requirements and avoid creating enclosed openings that are too small for reliable fabrication.

#### Commands Used

```tcl
drc check
drc why
drc find
```

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/minimum_area_rule_03a.png" width="100%">

**Figure 49.** Minimum area rule verification.

</td>

<td align="center" width="50%">

<img src="images/module_03/minimum_hole_rule_03b.png" width="100%">

**Figure 50.** Minimum hole rule verification.

</td>

</tr>
</table>

-
## Practical 04 – Well Rules
### Objective

Understand the **well-related design rules** in the SKY130 PDK, focusing on **well structures** and **deep N-well implementation**. These rules ensure proper device isolation, substrate biasing, and reliable fabrication of CMOS layouts.

#### Commands Used

```tcl
drc check
drc why
drc find
```

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/wells_rule_04a.png" width="100%">

**Figure 51.** Well rule verification.

</td>

<td align="center" width="50%">

<img src="images/module_03/deep_nwell_rule_04b.png" width="100%">

**Figure 52.** Deep N-well rule verification.

</td>

</tr>
</table>

---

## Practical 05 – Derived Layers

### Objective

Understand the concept of **derived layers** in the SKY130 PDK and their role in physical verification. This practical demonstrates standard derived layers, high-voltage derived layers, and the automatic generation of derived layers used internally by the DRC engine.

#### Commands Used

```tcl
drc check
drc why
drc find
```

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/derived_layers_05a.png" width="100%">

**Figure 53.** Standard derived layer verification.

</td>

<td align="center" width="50%">

<img src="images/module_03/derived_layer_high_voltage_05b.png" width="100%">

**Figure 54.** High-voltage derived layer verification.

</td>

</tr>
</table>

<br>

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/derived_layers_automatic_generation_05c.png" width="100%">

**Figure 55.** Automatic generation of derived layers during DRC verification.

</td>

<td align="center" width="50%">

</td>

</tr>
</table>
  
  ---

## Practical 06 – Parameterized Devices (PCell)

### Objective

Understand the implementation of **Parameterized Devices (PCell)** in the SKY130 PDK. This practical demonstrates how parameterized cells are generated, configured, and verified to ensure that device geometries satisfy the required physical design rules.

#### Commands Used

```tcl
drc check
drc why
drc find
```

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/parameterized_devices_06a.png" width="100%">

**Figure 56.** Parameterized device implementation.

</td>

<td align="center" width="50%">

<img src="images/module_03/parameterized_devices_06b.png" width="100%">

**Figure 57.** Parameterized device configuration and verification.

</td>

</tr>
</table>

<br>

<p align="center">
    <img src="images/module_03/parameterized_devices_06c.png" width="70%">
</p>

<p align="center">
    <em><b>Figure 58.</b> Final parameterized device layout satisfying SKY130 design rules.</em>
</p>

---

## Practical 07 – Angle Rules

### Objective

Understand the **angle-related design rules** in the SKY130 PDK. This practical demonstrates how Magic detects non-manufacturable geometries caused by improper angles and verifies layouts against the permitted angular constraints to ensure reliable fabrication.

#### Commands Used

```tcl
drc check
drc why
drc find
```

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/angle_rule_07a.png" width="100%">

**Figure 59.** Angle rule violation identified during DRC verification.

</td>

<td align="center" width="50%">

<img src="images/module_03/angle_rule_07b.png" width="100%">

**Figure 60.** Analysis of the angle rule violation using Magic.

</td>

</tr>
</table>

<br>

<p align="center">
    <img src="images/module_03/angle_rule_07c.png" width="70%">
</p>

<p align="center">
    <em><b>Figure 61.</b> Corrected layout satisfying the SKY130 angle rule.</em>
</p>

---

## Practical 08 – Seal Ring Verification

### Objective

Understand the purpose of the **seal ring** in the SKY130 PDK and verify the associated design rules using Magic. Seal rings are placed around the chip boundary to protect the die from mechanical damage, contamination, moisture, and process-induced stress during fabrication and packaging.

#### Commands Used

```tcl
drc check
drc why
drc find
```

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/seal_ring_08a.png" width="100%">

**Figure 62.** Initial seal ring layout and DRC verification.

</td>

<td align="center" width="50%">

<img src="images/module_03/seal_ring_08b.png" width="100%">

**Figure 63.** Analysis of seal ring rule violations.

</td>

</tr>
</table>

<br>

<p align="center">
    <img src="images/module_03/seal_ring_08c.png" width="70%">
</p>

<p align="center">
    <em><b>Figure 64.</b> Final seal ring layout satisfying the required SKY130 design rules.</em>
</p>


---

## Practical 09 – Latch-Up Rules

### Objective

Understand the **latch-up prevention rules** in the SKY130 PDK and verify the required spacing and well isolation constraints using Magic DRC. These rules help prevent parasitic PNPN structures that can create unintended current paths and lead to device failure.

#### Commands Used

```tcl
drc check
drc why
drc find
```

<p align="center">
    <img src="images/module_03/latchup_rules_09a.png" width="80%">
</p>

<p align="center">
    <em><b>Figure 65.</b> Latch-up rule verification using Magic DRC.</em>
</p>

## Practical 10 – Antenna Rules

### Objective

Understand the **antenna effect** in VLSI layouts and verify the corresponding **antenna rules** using Magic DRC. This practical demonstrates how antenna violations occur during fabrication, how they are identified, and the techniques used to eliminate them for reliable chip manufacturing.

#### Commands Used

```tcl
antennacheck
antennacheck debug
drc check
drc why
```

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/antenna_rules_10a.png" width="100%">

**Figure 66.** Antenna rule verification using Magic DRC.

</td>

<td align="center" width="50%">

<img src="images/module_03/antenna_rules_10b.png" width="100%">

**Figure 67.** Analysis of antenna rule violations.

</td>

</tr>
</table>

<br>

<p align="center">
    <img src="images/module_03/antenna_rules_10c.png" width="70%">
</p>

<p align="center">
    <em><b>Figure 68.</b> Corrected layout satisfying the SKY130 antenna rules.</em>
</p>


---

## Practical 11 – Density Rules

### Objective

Understand the **density verification** process in the SKY130 PDK and learn how metal fill patterns are generated to satisfy the minimum and maximum density requirements before tapeout. This practical demonstrates density analysis, automatic fill generation, and verification using Magic.

#### Commands Used

```tcl
cif cover MET2
```

```bash
/usr/share/pdk/sky130A/libs.tech/magic/check_density.py exercise_11.gds

/usr/share/pdk/sky130A/libs.tech/magic/generate_fill.py exercise_11.mag
```

```tcl
load exercise_11

box values 0 0 0 0

getcell exercise_11_fill_pattern child 0 0
```

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/density_rule_11a.png" width="100%">

**Figure 69.** Checking metal density using the `cif cover MET2` command.

</td>

<td align="center" width="50%">

<img src="images/module_03/density_rule_11b.png" width="100%">

**Figure 70.** Running the SKY130 density checking script.

</td>

</tr>
</table>

<br>

<table align="center">
<tr>

<td align="center" width="50%">

<img src="images/module_03/density_rule_11c.png" width="100%">

**Figure 71.** Automatic generation of metal fill patterns.

</td>

<td align="center" width="50%">

<img src="images/module_03/density_rule_11d.png" width="100%">

**Figure 72.** Loading the generated fill pattern into the layout.

</td>

</tr>
</table>

<br>

<p align="center">
    <img src="images/module_03/density_rule_11e.png" width="75%">
</p>

<p align="center">
    <em><b>Figure 73.</b> Final layout after inserting the generated fill pattern.</em>
</p>

**Learn More**

The complete density verification methodology and related design rules are available in the official SKY130 documentation:

https://skywater-pdk.readthedocs.io/en/main/rules.html

## Common Issues & Solutions

The following are some common issues encountered during the workshop along with the solutions that worked during implementation.

### Issue 1 – Primitive Cells Not Found During LVS

<p align="center">
    <img src="images/module_03/openpdk_integration_error.jpeg" width="900">
</p>

<p align="center">
    <em>Figure X. LVS error caused by incorrect SKY130 Open_PDK integration.</em>
</p>

**Problem**

While importing the SPICE netlist or running LVS, Magic/Netgen reports errors such as:

- Unknown cell `sky130_fd_pr_nfet_01v8`
- Unknown cell `sky130_fd_pr_pfet_01v8`
- Unable to read primitive cells

**Cause**

The SKY130 Open_PDK technology files were not linked correctly in the working directory.

**Solution**

- Create a fresh working directory for the lab.
- Copy the required SKY130 technology files into the new directory.
- Verify that the paths match your installation (they may differ depending on the environment).
- Import the SPICE netlist again before running LVS.

For example:

```bash
cp /usr/share/pdk/sky130A/libs.tech/magic/sky130A.magicrc ./.magicrc
```

> **Note:** Depending on the installation, the PDK may be located under `/usr/share/pdk/...` or `/usr/local/share/pdk/...`. Always verify the correct path before executing the commands.

---

### Issue 2 – Unexpected Paint Behavior in Magic

<p align="center">
    <img src="images/module_03/magic_paint_behavior_issue.jpeg" width="900">
</p>

<p align="center">
    <em>Figure X. Unexpected painting behavior while editing layouts in Magic.</em>
</p>

**Problem**

After cloning the Lab 3 repository and opening Magic, the layout entered paint mode unexpectedly, making normal editing difficult.

**Solution**

Simply:

1. Zoom out (`Shift + Z`)
2. Zoom back in (`Z`)

The layout window returns to normal operation without restarting Magic.

> This issue may occur occasionally while working in the VNC environment and can usually be resolved by refreshing the view through zooming.
> ---
