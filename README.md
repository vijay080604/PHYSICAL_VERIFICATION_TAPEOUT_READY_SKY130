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

This module introduces the physical verification environment using the **SKY130 Open PDK** and the open-source EDA toolchain. It covers the initial setup, technology files, and the complete physical verification workflow required before performing DRC and LVS.