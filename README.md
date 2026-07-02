# Physical Verification for Tapeout Readiness using SKY130

<p align="center">
  <img src="images/course_banner.png" alt="Physical Verification for Tapeout Readiness using SKY130" width="100%">
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

This module covers the installation of the open-source EDA tools required for physical verification using the **SKY130 Open PDK**. These tools are used throughout the workshop for layout design, simulation, extraction, DRC, and LVS verification.

### Magic

**Magic** is an open-source VLSI layout editor used for layout creation, editing, extraction, and Design Rule Checking (DRC).

**Installation**

```bash
git clone git://opencircuitdesign.com/magic
cd magic
./configure
make
sudo make install
```

| Command | Purpose |
|:--------|:--------|
| `git clone` | Downloads the Magic source code. |
| `cd magic` | Moves into the Magic source directory. |
| `./configure` | Configures the build environment. |
| `make` | Compiles the source code. |
| `sudo make install` | Installs Magic on the system. |

**Official Documentation:** http://opencircuitdesign.com/magic/index.html

---

### Netgen

**Netgen** is an open-source Layout Versus Schematic (LVS) tool used to compare extracted layout netlists with schematic netlists.

**Installation**

```bash
git clone git://opencircuitdesign.com/netgen
cd netgen
./configure
make
sudo make install
```

| Command | Purpose |
|:--------|:--------|
| `git clone` | Downloads the Netgen source code. |
| `cd netgen` | Opens the Netgen directory. |
| `./configure` | Configures the build environment. |
| `make` | Compiles Netgen. |
| `sudo make install` | Installs Netgen. |

**Official Documentation:** http://opencircuitdesign.com/netgen/index.html

---

### Xschem

**Xschem** is an open-source schematic capture tool used for creating circuit schematics before layout implementation.

**Installation**

```bash
git clone https://github.com/StefanSchippers/xschem.git xschem_git
cd xschem_git
./configure
make
sudo make install
```

| Command | Purpose |
|:--------|:--------|
| `git clone` | Downloads the Xschem repository. |
| `cd xschem_git` | Opens the Xschem directory. |
| `./configure` | Prepares the build configuration. |
| `make` | Compiles Xschem. |
| `sudo make install` | Installs Xschem. |

**Official Documentation:** http://repo.hu/projects/xschem/index.html

---

### ngspice

**ngspice** is an open-source circuit simulator used for analog and mixed-signal circuit simulation.

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

| Command | Purpose |
|:--------|:--------|
| `tar -zxvf` | Extracts the downloaded package. |
| `mkdir release` | Creates a separate build directory. |
| `../configure` | Configures the build options. |
| `make` | Compiles ngspice. |
| `sudo make install` | Installs ngspice. |

To display simulation waveforms, install **xterm**:

```bash
sudo apt-get update
sudo apt-get install xterm
```

**Official Documentation:** https://ngspice.sourceforge.io/index.html

---

### Open_PDKs

**Open_PDKs** provides the complete **SKY130 Open PDK** environment required by open-source EDA tools such as Magic, Netgen, KLayout, and OpenLANE.

**Installation**

```bash
git clone git://opencircuitdesign.com/open_pdks
cd open_pdks
./configure --enable-sky130-pdk
make
sudo make install
```

| Command | Purpose |
|:--------|:--------|
| `git clone` | Downloads the Open_PDKs repository. |
| `cd open_pdks` | Opens the project directory. |
| `./configure --enable-sky130-pdk` | Enables the SKY130 PDK during configuration. |
| `make` | Builds the PDK. |
| `sudo make install` | Installs the SKY130 PDK. |

**Official Repository:** https://github.com/RTimothyEdwards/open_pdks

---

With the required tools successfully installed, the physical verification environment is ready. The next step is to understand the complete physical verification design flow before moving on to the laboratory exercises.

<p align="right">
  <a href="#repository-navigation">Back to Navigation ↑</a>
</p>