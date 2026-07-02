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
<em>Figure 3. Xschem successfully configured with SKY130 PDK.</em>
</td>

<td align="center">
<img src="images/module_01/ngspice_working.png" width="420"><br>
<em>Figure 4. ngspice successfully configured with SKY130 PDK.</em>
</td>
</tr>

<tr>
<td align="center">
<img src="images/module_01/magic_working.png" width="420"><br>
<em>Figure 5. Magic successfully loaded with the SKY130 technology file.</em>
</td>

<td align="center">
<img src="images/module_01/netgen_working.png" width="420"><br>
<em>Figure 6. Netgen successfully configured for LVS using SKY130.</em>
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
    <em>Figure 7. CMOS inverter schematic created in Xschem using the SKY130 device library.</em>
</p>

---

### Step 2 – Circuit Simulation

A dedicated testbench is created using the generated inverter symbol to verify the circuit operation in **ngspice**. The voltage transfer characteristics (VTC) confirm the expected switching behaviour of the CMOS inverter.

<p align="center">
    <img src="images/module_01/inverter_tb_schematic.png" width="900">
</p>

<p align="center">
    <em>Figure 8. Testbench schematic used for CMOS inverter simulation.</em>
</p>

<p align="center">
    <img src="images/module_01/ngspice_with_vtc_charateristics.png" width="900">
</p>

<p align="center">
    <em>Figure 9. ngspice simulation showing the generated VTC characteristics.</em>
</p>

<p align="center">
    <img src="images/module_01/cmos_inverter_vtc_output.png" width="700">
</p>

<p align="center">
    <em>Figure 10. CMOS inverter Voltage Transfer Characteristic (VTC).</em>
</p>

> **Observation:** The simulation confirms the expected inverter switching behaviour before proceeding to physical layout implementation.

---

### Step 3 – Physical Layout Implementation

The verified schematic is implemented as a physical layout in **Magic** using the SKY130 technology. Device placement and routing are completed before extracting the layout netlist.

<p align="center">
    <img src="images/module_01/layout1_cmos_inverter.png" width="900">
</p>

<p align="center">
    <em>Figure 11. CMOS inverter layout implemented using Magic.</em>
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
    <em>Figure 12. Layout extraction completed successfully.</em>
</p>

<p align="center">
    <img src="images/module_01/spice_extraction.png" width="900">
</p>

<p align="center">
    <em>Figure 13. Extracted SPICE netlist generated from the Magic layout.</em>
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
    <em>Figure 14. Executing LVS verification using Netgen.</em>
</p>

<p align="center">
    <img src="images/module_01/lvs_result.png" width="900">
</p>

<p align="center">
    <em>Figure 15. LVS comparison result generated by Netgen.</em>
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
    <em>Figure 1. Overview of the physical verification flow from schematic design to layout verification using DRC and LVS.</em>
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
    <em>Figure 2. Copying the SKY130 Magic technology file and importing the standard-cell GDS library.</em>
</p>

<p align="center">
    <img src="images/module_02/reading_gds_cell_02.png" width="900">
</p>

<p align="center">
    <em>Figure 3. Reading the SKY130 standard-cell GDS database into Magic.</em>
</p>

<p align="center">
    <img src="images/module_02/selection_and_2_cell_03.png" width="900">
</p>

<p align="center">
    <em>Figure 4. Selecting and loading the <code>sky130_fd_sc_hd__and2_1</code> standard cell.</em>
</p>

<p align="center">
    <img src="images/module_02/and_2_1_cell_layout_view_04.png" width="900">
</p>

<p align="center">
    <em>Figure 5. Layout view of the <code>sky130_fd_sc_hd__and2_1</code> standard cell in Magic.</em>
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
    <em>Figure 6. Configuring port information for the standard cell.</em>
</p>

<p align="center">
    <img src="images/module_02/06_lef_import.png" width="900">
</p>

<p align="center">
    <em>Figure 7. Importing LEF data for the SKY130 standard-cell library.</em>
</p>
<p align="center">
    <img src="images/module_02/read_spice_library_07.png" width="900">
</p>

<p align="center">
    <em>Figure X. SPICE library loaded successfully, providing the netlist information required for LVS verification.</em>
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
    <em>Figure 6. Placing the <code>sky130_fd_sc_hd__and2_1</code> standard cell as an instance using <code>getcell</code>.</em>
</p>

<p align="center">
    <img src="images/module_02/gds_write_09.png" width="900">
</p>

<p align="center">
    <em>Figure 7. Making the imported standard cell writable before exporting the updated layout.</em>
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
    <em>Figure 8. Extracting the layout and generating an LVS-compatible SPICE netlist.</em>
</p>

<p align="center">
    <img src="images/module_02/extracted_spice_netlist_11.png" width="900">
</p>

<p align="center">
    <em>Figure 9. SPICE netlist generated from the extracted layout.</em>
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
    <em>Figure 10. Generating the extracted SPICE netlist with capacitance information.</em>
</p>
<p align="center">
    <img src="images/module_02/extracted_spice_netlist_of_cthresh_13.png" width="900">
</p>

<p align="center">
    <em>Figure 9. SPICE netlist generated from the extracted layout.</em>
</p>
<p align="center">
    <img src="images/module_02/spice_resistance_extraction_14.png" width="900">
</p>

<p align="center">
    <em>Figure 10. Generating the extracted SPICE netlist with resistance information.</em>
</p>
</p>
<p align="center">
    <img src="images/module_02/extraction_files_15.png" width="900">
</p>
<p align="center">
    <em>Figure 11.files created after all extraction operations</em>
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
    <em>Figure 14. Running Design Rule Check (DRC) on the SKY130 standard cell.</em>
</p>

<p align="center">
    <img src="images/module_02/drc_violation_16.png" width="900">
</p>

<p align="center">
    <em>Figure 15. Viewing DRC results and locating rule violations in Magic.</em>
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
    <em>Figure 16. Executing Layout Versus Schematic (LVS) verification using Netgen.</em>
</p>

<p align="center">
    <img src="images/module_02/lvs_result_19.png" width="900">
</p>

<p align="center">
    <em>Figure 17. LVS comparison between the extracted layout netlist and the reference SPICE netlist.</em>
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
    <em>Figure 18. Performing XOR verification to compare layout differences.</em>
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
