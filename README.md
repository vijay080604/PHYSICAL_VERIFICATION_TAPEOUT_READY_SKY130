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
---

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