# VSDSquadron-RTL2GDS-SoC-Implementation
> 6 Weeks RTL2GDS SoC Implementation programme by VSD which focuses on System &amp; RTL Foundations to Physical Design and sign-Off using OPENLANE

![Static Badge](https://img.shields.io/badge/OS-linux-orange)
![Static Badge](https://img.shields.io/badge/EDA%20Tools-OpenLANE--Flow%2C_Yosys%2C_abc%2C_OpenROAD%2C_TritonRoute%2C_OpenSTA%2C_magic%2C_netgen%2C_GUNA-navy)
![Static Badge](https://img.shields.io/badge/languages-verilog%2C_bash%2C_TCL-crimson)

![OpenROAD](https://img.shields.io/badge/OpenROAD-EDA-blue?style=for-the-badge)
![SKY130](https://img.shields.io/badge/PDK-SKY130-green?style=for-the-badge)
![Verilog](https://img.shields.io/badge/Verilog-HDL-red?style=for-the-badge)
![Linux](https://img.shields.io/badge/Linux-Ubuntu-black?style=for-the-badge)



***The curriculum of this workshop will be carried out in different phases and will be completed in 6 weeks.***

# WEEK 1

In WEEK 1, we will be deep diving into Digital VLSI SoC Design and Planning, which is the foundation of this workshop.

Now Let’s begin with a simplified RTL2GDSII Flow which is given below:


<img width="999" height="304" alt="image" src="https://github.com/user-attachments/assets/d4d4853d-d09e-4123-aa95-e4294c988c51" />

 


Now what is PDK?

* The main motto of our work is to make our design manufacturable. The place where designs are realised physically in silicon wafer is called Fab or fabrication Facility.
* So, while designing any chip we need to consider some rules imposed by fabrication facility to make our chip physically realisable.
* PDK stands for Process Design Kit, which act as an interface between designer and fabrication facility. It is a set of files and tools used in the semiconductor industry to model a foundry's manufacturing process inside an EDA tool.
* In our entire design process, we will use the same required PDK.

# Now moving to RTL2GSDII Flow,

Once we have the required RTL Code, the first step is to synthesize the RTL.

* **Synthesis**: This process gives us the optimised gate level netlist.

* **Floor planning and power planning**: In this step, we will define the aspect ratio of the core, Place the Macros and De-cap cells, carryout power planning, place pre-placement cells, define pin location.

* **Placement**: In this stage we will place the standard cell in standard cell Row site and legalize their placement.

* **CTS**: Clock tree synthesis is the stage where we will give clock signal to every block in a way that we have minimum skew and low latency.

* **Routing**: Routing is making physical connections between signal pins using metal layers.

    It can be divided into two steps:

     a)	**Global Routing**: This step identifies the possible routing tracks.

     b)	**Detailed Routing**: This step physically connects the signal pins.

* **Sign-Off**: This steps checks whether there is any DRC violations or not, carry out schematic vs LVS check, and perform STA analysis.


The entire design will be done using OpenLane, which is a flow and comprised of many open-source EDA tools like Yosis+abc,magic,Ngspice etc.

The main goal is to have a clean GDSII without human intervention.

OpenLane ASIC flow diagram is given below:

 
<img width="1081" height="615" alt="image" src="https://github.com/user-attachments/assets/f11c5bef-0c0f-438d-bc2e-1dfde73e6681" />


In this Week all the labs will be carried out in GitHub Codespaces (vsd-openlane).

The below link provides steps to setup github codepsace:

https://github.com/vsdip/vsd-openlane

<details>
<summary>Day Phase 1 — OpenLANE Flow Familiarity (RTL → Synthesis literacy) </summary>
 We will run “picorv32a” design synthesis using OpenLANE flow and generate necessary outputs.

### Commands to invoke the OpenLANE flow and perform synthesis:

```bash 
# Change directory to openlane flow directory
cd /home/vscode/Desktop/openlane

# Since we have aliased the long command to "make mount" we can invoke the OpenLANE flow docker sub-system by just running this command
make mount

```


 ```tcl
#	Once noVNC Desktop is opened, go to terminal ->
  cd OpenLane

 # to invoke OpenLane flow  
 make mount

#we can invoke the OpenLANE flow in the Interactive mode using this command
   ./flow.tcl  -interactive

#required packages for proper functionality of the OpenLANE flow
   package require openlane

# As OpenLANE flow is ready to run any design and initially we have to prepare the design creating some necessary files and we run this command and our design name is picorv32a
   prep -design picorv32a

# To synthesize the RTL code of the design
 run_synthesis 
    
# 	To get out of the OpenLane flow type
 exit.
```

    

Screenshots are attached below:

<img width="1004" height="596" alt="image" src="https://github.com/user-attachments/assets/3b1a5e32-cb00-41e3-a732-ace4e5628fef" />

<img width="940" height="641" alt="image" src="https://github.com/user-attachments/assets/200871bb-c7ca-451a-851c-21c539eb5cba" />

<img width="940" height="694" alt="image" src="https://github.com/user-attachments/assets/0431aeef-a4e7-43b4-b7c1-08079d1f489f" />

Calculation of Flop Ratio and DFF % from synthesis statistics report file

```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}

```
```math
Percentage\ of\ DFF's = Flop\ Ratio * 100

```
```math
Flop\ Ratio = \frac{1613}{18508} = 0.0871515
```
```math
Percentage\ of\ DFF's = 0.0871515 * 100 = 8.71515\ \%
```



</details>
 


## Phase 2 — Floorplan Fundamentals (macro awareness for Caravel blocks)

In this phase we will look into some Floorplan concepts and run floorplan in Openlane.

## Utilization factor and aspect ratio:

* **Aspect ratio:** The ratio of the height of the chip core to its width.

<img width="551" height="175" alt="image" src="https://github.com/user-attachments/assets/72aca877-071f-4f47-8249-fa6a0e63ff4c" />


* **Utilisation factor:** Ratio of  Area occupied by standard cell to the total core Area.

## Concept of MACROS:

* A macro is a large pre-designed functional block used in chip design that is treated as a single unit during placement and routing.
* Unlike standard cells, macros are pre-designed, Pre-characterised, Pre-verified and larger in size.
* Macros ae placed manually during floor planning.


## A short explanation (your own words): why macros like RAM behave differently than standard cells:

  a) RAM macros behave differently from standard cells because they are large, custom-designed memory blocks treated as fixed black boxes in the design flow, with timing and power dominated by internal memory array effects rather than simple gate delay.

  b) Standard cells Can be resized (different drive strengths) whereas macros are of fixed size and cannot be resized.

  c) During synthesis, standard cells can be optimised but macros cannot be optimised.

Screenshots are attached below:

# Invoking floor plan command

![Screenshot 2026-02-23 001813](https://github.com/user-attachments/assets/11cf4eb6-11bb-49b1-b5a7-1e964da13788)

System default configuration

![Screenshot 2026-02-23 002636](https://github.com/user-attachments/assets/c2777f19-e4a5-4765-9561-fa5354c725c8)

![Screenshot 2026-02-23 002517](https://github.com/user-attachments/assets/0d968735-ae9e-402d-96c5-e11019328099)

Config.tcl details inside picorv32a folder

![Screenshot 2026-02-23 003007](https://github.com/user-attachments/assets/12c19ddc-e35d-4594-8194-06602646cbad)

With this config.tcl we get floorplan output as:

![Screenshot 2026-02-23 010111](https://github.com/user-attachments/assets/5eded601-6006-4610-b614-053ff9c8adee)




Floor plan def file with this config.tcl file:

![Screenshot 2026-02-23 004222](https://github.com/user-attachments/assets/3c6dc64f-c2be-4840-b932-ff1689b91d37)

Once we add the sky130A_sky130_fd_sc_hd_config.tcl file and to have its impact on design, that is to overwrite the config.tcl we need to modify the config.tcl as below:

sky130A_sky130_fd_sc_hd_config.tcl file

![Screenshot 2026-02-24 230718](https://github.com/user-attachments/assets/8d67aa2f-7eea-4589-aa10-2a69e85abf74)

Modified config.tcl:

![Screenshot 2026-02-24 230816](https://github.com/user-attachments/assets/1e9622b9-a429-462c-b3a2-f1a994330dce)

With this,we get floorplan output as:

![Screenshot 2026-02-24 230213](https://github.com/user-attachments/assets/ed2d3ebf-8a5e-4331-af3d-1d22869b2ba9)

Floorplan def after adding the sky130A_sky130_fd_sc_hd_config.tcl file

![Screenshot 2026-02-24 231559](https://github.com/user-attachments/assets/1e3b4ab8-ee5d-4e91-9568-860676879f47)




**Layout in  Magic**

Coomands to view def in Magic:
```tcl
cd /home/vscode/Desktop/Openlane/designs/picorv32a/runs/RUN_2026.02.27_17.20.00/results/floorplan/
magic -T /home/vscode/.ciel/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.max.lef def read picorv32a.def

```

![Screenshot 2026-02-23 013235](https://github.com/user-attachments/assets/83fc78a3-0b93-4ad8-8aa5-0c209a4e2b29)

Pin Details:
![Screenshot 2026-02-23 013402](https://github.com/user-attachments/assets/b01223ee-dc3d-45f9-b980-d903280f71c5)

![Screenshot 2026-02-23 013439](https://github.com/user-attachments/assets/60a4ec04-b069-4176-bb51-269020d6a212)

![Screenshot 2026-02-23 013816](https://github.com/user-attachments/assets/0052f0da-c8a0-4bb8-b9c4-e4318ecd2f97)

![Screenshot 2026-02-23 012200](https://github.com/user-attachments/assets/f7b41943-bdda-4502-975d-bc829f05d54b)

## Phase 3 — Timing Literacy with Ideal Clocks (OpenSTA + ECO mindset)

Timing checks like setup and Hold time checks are carried out to ensure that timing constraints are met.

* **Setup Time**: It is the minimum amount of time required for the data signal to be remain stable before the arrival of active edge of the clock.

* **Hold Time**: It is the minimum amount of time required for the data signal to be remain stable after the arrival of active edge of the clock.

To run STA, we need a **pre_sta.conf** file but it is not available. So we have created a file:

![Screenshot 2026-02-26 011455](https://github.com/user-attachments/assets/a6f135b2-9d40-4d44-acaf-06c1456a5f84)

Then we have copied lib file in picorv32afolder inside src.

Then we run STA using command
```tcl

 sta pre_sta.conf

```
 ![Screenshot 2026-02-26 004100](https://github.com/user-attachments/assets/d736332b-9d21-4a2b-a916-20dd4c0e4c4d)


**Hold Slack details:**

![Screenshot 2026-02-26 004126](https://github.com/user-attachments/assets/921960f0-d266-4ead-a8b0-9014030d55fb)

**Setup slack details:**

![Screenshot 2026-02-26 004246](https://github.com/user-attachments/assets/eae59b26-4c08-48ef-b0b1-1095ef10fa08)

![Screenshot 2026-02-26 004203](https://github.com/user-attachments/assets/aebfd2f0-d198-4432-8919-f7aea1a1273b)


To improve the slack we can either increase buffer strength or we can reduce fanout.

![Screenshot 2026-02-27 012455](https://github.com/user-attachments/assets/b0521f6c-6323-418f-a192-b6557b9f7280)

![Screenshot 2026-02-27 022228](https://github.com/user-attachments/assets/9014b66a-965c-4f0d-9839-3aa577e651dc)

![Screenshot 2026-02-27 022303](https://github.com/user-attachments/assets/69523f8d-9aa3-4832-acce-2dc17a21d502)

![Screenshot 2026-02-27 022847](https://github.com/user-attachments/assets/8b82cfc3-e4d3-4f8a-8b30-91e57ff28f65)

![Screenshot 2026-02-27 022326](https://github.com/user-attachments/assets/42e76075-c01f-4830-b19c-d405f59a8660)

![Screenshot 2026-02-27 023710](https://github.com/user-attachments/assets/bc1113a7-e963-4e3e-aad5-3d8299e4d5a7)

![Screenshot 2026-02-27 025637](https://github.com/user-attachments/assets/2a35c9e3-9d02-494c-b6cf-ef7900bf8337)

![Screenshot 2026-02-27 025704](https://github.com/user-attachments/assets/b0de2d79-dff9-4176-ab74-b00099b71269)

We can observe that slack is improved.

# Phase 4 — CTS and Timing with Real Clocks

CTS run proof screenshot:

![Screenshot 2026-02-27 225631](https://github.com/user-attachments/assets/2eebafcd-d353-479c-bcfb-11d047334636)

![Screenshot 2026-02-27 225658](https://github.com/user-attachments/assets/bce7f45e-b971-4091-816a-087f774eb532)

# Post-CTS OpenROAD timing analysis.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD:
```tcl

openroad

read_lef /home/vscode/OpenLane/designs/picorv32a/runs/26.02.27-17.20.00/tmp/merged.max.lef

read_def  /home/vscode/OpenLane/designs/picorv32a/runs/26.02.27-17.20.00/results/cts/picorv32a.def

write_db pico_cts.db

read_db pico_cts.db

read_verilog / home/vscode/OpenLane/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis.v

read_liberty $::env(LIB_SYNTH_COMPLETE)

link_design picorv32a

read_sdc /openLANE_flow/designs/picorv32a/src/picorv32a.sdc

set_propagated_clock [all_clocks]

report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

```

**HOLD Slack:**

![Screenshot 2026-02-26 004126](https://github.com/user-attachments/assets/8ecb4745-9990-4fa0-8633-72d092ef56ab)

**SETUP Slack:**

![Screenshot 2026-02-27 231602](https://github.com/user-attachments/assets/3ee20893-8520-4777-b416-a6a03d75f36a)

![Screenshot 2026-02-27 231531](https://github.com/user-attachments/assets/38397e35-ffee-441c-be62-d2659679a77d)

# Phase 5 — PDN Awareness (required vocabulary for ORFS and signoff thinking)

For power routing, higher metal layer is used for routing. In higher metal layer resistance is low due higher metal area. So IR drop is reduced.

**Invoking PDN:**

After cts stage is completed, we can invoke gen_pdn command for power routing.

![Screenshot 2026-02-28 212252](https://github.com/user-attachments/assets/bca7c03f-519a-4035-8598-b33ca9e1f145)


To view in magic, following commands are provided:
```tcl
cd /home/vscode/Desktop/Openlane/designs/picorv32a/runs/RUN_2026.02.27_17.20.00/results/floorplan/
magic -T /home/vscode/.ciel/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.max.lef def read picorv32a.def

```



<img width="1245" height="163" alt="image" src="https://github.com/user-attachments/assets/0bc9455b-91b7-4335-bb1b-e206a4ace8bd" />


Screenshot:

![Screenshot 2026-02-28 212638](https://github.com/user-attachments/assets/aef26461-ccc9-4ce4-b1a2-be5a5e444475)

![Screenshot 2026-02-28 212742](https://github.com/user-attachments/assets/4ec39637-1cba-4bbe-b903-0869623d008c)

![Screenshot 2026-02-28 213226](https://github.com/user-attachments/assets/1cb45248-6a8f-4b77-886c-1791b32604ed)

![Screenshot 2026-02-28 213313](https://github.com/user-attachments/assets/76dd1663-7c15-4f50-9042-7aefa56eeece)

![Screenshot 2026-02-28 213445](https://github.com/user-attachments/assets/5b23ef2e-b78d-41d1-aabe-1afbca355063)


 PDN must be created before routing because it defines the power backbone of the chip.
 
-  It reserves metal resources that signal routing must avoid.
   
- If PDN is added after routing, signals will be ripped up and rerouted.
  
- PDN blocks routing tracks, so congestion must be planned early.
  
- Early PDN allows early IR drop estimation and correction.
  
- Weak PDN causes voltage drop, which increases cell delay.
 
- Increased delay leads to setup timing violations at signoff.
  
- Poor PDN can also cause electromigration (EM) failures.
  
 - EM issues cannot be fixed easily at late stages without major redesign.

 - Stable PDN ensures predictable timing and reliable signoff. Since IR drop, EM, and timing are signoff checks, PDN quality directly affects signoff success. Therefore, PDN      must be built before placement and routing to avoid late-stage ECOs and timing instability.



# WEEK–2

**Toolchain Mastery and ORFS Execution (Cloud to Local)**

Week-2 Focuses on:

1.	Running ORFS (OpenROAD Flow Scripts) in GitHub Codespaces
2.	Understanding the complete toolchain inside the devcontainer
3.	Installing ORFS locally
4.	Installing official OpenROAD locally
5.	Re-executing the same testcase locally
6.	Developing Unix and environment debugging literacy

# PHASE 1 — ORFS Execution in GitHub Codespaces

## Task 1.1 — Repository Setup

1. First we fork the repository from https://github.com/vsdip/vsd-scl180-orfs
2. Then we run	and Launch GitHub Codespaces.
3. Then 	allow devcontainer to build fully
4. Fianlly we	Confirmed that container environment is ready to use.

**Screenshot**
![Screenshot 2026-03-02 230255](https://github.com/user-attachments/assets/752e8638-f1dd-4447-81a8-2c9d02015358)

![Screenshot 2026-03-02 230445](https://github.com/user-attachments/assets/f2a1e0d3-e839-4198-a713-ce5a5e297f2f)

![Screenshot 2026-03-07 222615](https://github.com/user-attachments/assets/bdd48656-7de7-4b34-af7c-92db1bc18e05)

# Task 1.2 — Run Sky130 Testcase in Cloud

## Full RTL-to-GDS flow using the provided Makefile

Before starting we choose the pdk as sky130 replacing scl180 in Makefile.

vim Makefile

![Screenshot 2026-03-04 130555](https://github.com/user-attachments/assets/e2d91ef0-849c-4ec7-8283-ce74b397fc11)


 ## Synthesis:
  ```bash
make synth
# To view Gui
make gui_synth
```
 
  ![Screenshot 2026-03-04 143815](https://github.com/user-attachments/assets/1c153540-31ea-486c-bb83-aad9c650afac)

  ![Screenshot 2026-03-04 144622](https://github.com/user-attachments/assets/145d1549-36c9-4a60-886a-2dbcf9ad5d6c)

  ![Screenshot 2026-03-04 144604](https://github.com/user-attachments/assets/babe4a08-8e6e-4ddb-8678-bd5dd250bfe2)

  ## Floorplan:

  ```bash
make floorplan
# To view Gui
make gui_floorplan
```

  ![Screenshot 2026-03-04 144851](https://github.com/user-attachments/assets/b13beac6-59b2-4c5e-9c8a-886f635351fb)

  ![Screenshot 2026-03-04 145002](https://github.com/user-attachments/assets/8ee42d65-5318-4d53-9040-c83a10c245f2)

  ![Screenshot 2026-03-04 144945](https://github.com/user-attachments/assets/20f3a486-b0a3-43c1-ae86-2e9a50596760)

## Placement:

```bash
make place
# To view Gui
make gui_place
```

![Screenshot 2026-03-04 145351](https://github.com/user-attachments/assets/869b214c-48b0-4876-885f-6f22ad325d1c)

![Screenshot 2026-03-04 145748](https://github.com/user-attachments/assets/64ea907a-e181-4971-9032-c140a1823a9b)

![Screenshot 2026-03-04 145424](https://github.com/user-attachments/assets/f9cc22ef-f179-49c7-b109-64c2a7a9c0cd)

![Screenshot 2026-03-04 145458](https://github.com/user-attachments/assets/6f033cd7-ba83-4705-bd1f-3ce1b48a0437)

![Screenshot 2026-03-04 145523](https://github.com/user-attachments/assets/f8718c15-8354-4b14-92b5-91d069bef758)

# CTS:

```bash
make cts
# To view Gui
make gui_cts
```

![Screenshot 2026-03-04 150052](https://github.com/user-attachments/assets/a95a1e1f-50f5-410f-9eb9-aeda33d1e468)

![Screenshot 2026-03-04 151214](https://github.com/user-attachments/assets/58b5f3db-cd3b-449d-9a4f-5d845a6373fd)

![Screenshot 2026-03-04 150500](https://github.com/user-attachments/assets/5311b3a9-97d9-4884-af55-6655e413074c)

![Screenshot 2026-03-04 150544](https://github.com/user-attachments/assets/39b82ac1-8703-49a1-9b76-5dae5376051c)

![Screenshot 2026-03-04 150758](https://github.com/user-attachments/assets/6db735c2-e4bb-42a6-a9f6-18bc6e971294)

# Route:

```bash
make route
# To view Gui
make gui_route
```

![Screenshot 2026-03-04 155755](https://github.com/user-attachments/assets/09f5ea30-7c95-44b3-a693-2cb50f150433)

![Screenshot 2026-03-04 155848](https://github.com/user-attachments/assets/79f0ec3e-8d9c-455d-b794-629d015e00af)

![Screenshot 2026-03-04 155924](https://github.com/user-attachments/assets/5543b2e8-bbe2-4ec8-bdf7-70d8370134c2)

![Screenshot 2026-03-04 160023](https://github.com/user-attachments/assets/5fa435d6-4fe6-499c-919e-cc5d91d843a3)

![Screenshot 2026-03-04 160102](https://github.com/user-attachments/assets/b2d47122-d8b8-4edd-b2a4-c453d730ffe1)

![Screenshot 2026-03-04 160141](https://github.com/user-attachments/assets/49f2eb7f-ce1a-486e-aa09-7ea72b30c16c)


# To generate Final report following is the command:

```bash
make final

# To view gds file

make gui_6_merged.gds   or
klayout 6_merged.gds

```
![Screenshot 2026-03-04 161651](https://github.com/user-attachments/assets/9062aa15-be51-4446-9dcc-d5547ff7dbce)

![Screenshot 2026-03-04 161947](https://github.com/user-attachments/assets/270ba0a6-6145-4de5-a966-a2019bdf78c7)

![Screenshot 2026-03-04 194039](https://github.com/user-attachments/assets/0791824e-c9c4-43cc-a0ce-60dd12b6aed3)

# PHASE 2 — Toolchain Understanding (Devcontainer Deep Dive)

Based on a standard OpenROAD devcontainer setup (Dockerfile + install-openroad.sh), below is the structured Toolchain Mapping for RTL-to-GDS flow.

🔧 RTL-to-GDS Toolchain Mapping (Dockerfile + install-openroad.sh)
Tool	Version Source	How Installed	Purpose in RTL → GDS Flow
OpenROAD	GitHub clone (OpenROAD repo)	Compiled from source using CMake	Core physical design engine. Performs floorplanning, placement, CTS, routing, optimization, and generates final DEF/GDS.
Yosys	GitHub clone or package manager	Usually compiled from source	RTL synthesis tool. Converts Verilog RTL → gate-level netlist (mapped to standard cells).
TritonCTS	Built as part of OpenROAD	Compiled within OpenROAD build	Clock Tree Synthesis (CTS). Inserts clock buffers to reduce skew and balance clock delays.
FastRoute	Integrated inside OpenROAD	Compiled as OpenROAD module	Global routing engine. Assigns routing tracks before detailed routing.
OpenSTA	GitHub clone (Parallax/OpenSTA) or bundled	Compiled from source	Static Timing Analysis (STA). Checks setup/hold violations pre- and post-route.
KLayout	Package manager (apt) or official binary	Installed via apt-get	Layout viewer. Used to visualize DEF/GDS and perform DRC/LVS checks.
Python	Package manager	apt-get install python3	Used for scripting flow automation, PDK setup, OpenROAD flow control scripts.
GNU Make	Package manager	apt-get install make	Builds OpenROAD and manages flow targets (make flow, make build, etc.).
Git	Package manager	apt-get install git	Clones OpenROAD, Yosys, OpenSTA repositories from GitHub.
________________________________________
📌 Tool Roles in Complete RTL-to-GDS Flow
1️⃣ RTL Synthesis Stage
•	Yosys
•	Output: Gate-level netlist (.v mapped to std cells)
2️⃣ Floorplanning & Placement
•	OpenROAD
•	Performs:
o	Floorplan creation
o	IO placement
o	Standard cell placement
o	Optimization
3️⃣ Clock Tree Synthesis
•	TritonCTS (inside OpenROAD)
•	Inserts clock buffers
•	Reduces skew and insertion delay
4️⃣ Routing
•	FastRoute → Global Routing
•	OpenROAD detailed router → Detailed Routing
5️⃣ Timing Analysis
•	OpenSTA
•	Checks:
o	Setup/hold timing
o	Slack
o	Path analysis
6️⃣ Signoff & Visualization
•	KLayout
•	Used to:
o	View GDS
o	Inspect metal layers
o	Run DRC/LVS (if configured)
Flow Dependency Diagram (Conceptual)
RTL (Verilog)
   ↓
Yosys (Synthesis)
   ↓
Gate-level Netlist
   ↓
OpenROAD (Floorplan + Placement)
   ↓
TritonCTS (Clock Tree)
   ↓
FastRoute (Global Routing)
   ↓
Detailed Routing
   ↓
OpenSTA (Timing Signoff)
   ↓
GDS
   ↓
KLayout (Visualization)
________________________________________
🔎 Important Observations from Dockerfile Pattern
Typical OpenROAD devcontainer:
•	Uses Ubuntu base image
•	Installs build dependencies:
o	gcc/g++
o	cmake
o	swig
o	bison
o	flex
•	Clones OpenROAD repo
•	Builds using:
mkdir build
cd build
cmake ..
make -j$(nproc)
This confirms:
•	OpenROAD, OpenSTA, TritonCTS, FastRoute → compiled from source
•	Python, Make, Git → installed via apt
•	KLayout → apt or prebuilt binary

## Task 2.2 — Flow Architecture Explanation


Flow Architecture Explanation (RTL → GDS using ORFS)

1️⃣ What ORFS Automates
OpenROAD-flow-scripts (ORFS) automates the complete RTL-to-GDSII digital implementation flow.
In simple words, ORFS acts like a flow manager that connects all EDA tools together and runs them in the correct sequence without manual intervention.
Instead of manually running:
•	synthesis
•	floorplanning
•	placement
•	clock tree synthesis
•	routing
•	timing checks
•	GDS generation
ORFS provides:
•	Predefined directory structure
•	Technology (PDK) integration
•	Automated script sequencing
•	Constraint handling
•	Report generation
•	Reproducible builds
So essentially, ORFS converts:
RTL + Constraints + PDK
        ↓
Fully placed, routed, timing-verified GDSII
It ensures consistency, automation, and minimal human error in physical design flow execution.
________________________________________
2️⃣ How Makefiles Orchestrate the Flow
ORFS uses Makefiles as the control engine of the flow.
Make works using:
•	Targets
•	Dependencies
•	Rules
Each major flow stage is a Make target, such as:
make synth
make floorplan
make place
make cts
make route
make final
🔹 How It Works Internally
•	If you run make, it checks:
o	What files already exist
o	What dependencies are outdated
•	It executes only the required steps.
•	Each stage generates outputs used by the next stage.
For example:
synth → produces netlist.v

place → depends on netlist.v

cts → depends on placed DEF

route → depends on CTS DEF

So Make ensures:
•	Correct execution order
•	Incremental builds
•	Flow reproducibility
•	Dependency tracking

In short, Makefiles act as the flow scheduler and dependency manager.
________________________________________

3️⃣ Where Synthesis Ends and Physical Design Begins?

🔹 Synthesis Stage

Performed by:
•	Yosys
Input:
•	RTL (Verilog)
•	Timing constraints (.sdc)
•	Liberty (.lib)
Output:
•	Gate-level netlist mapped to standard cells
At this stage:
•	No placement
•	No routing
•	No physical coordinates
•	Purely logical structure
________________________________________
🔹 Physical Design Begins
Physical design begins when:
•	The synthesized netlist is imported into OpenROAD
•	Floorplan is created
•	Standard cells get physical locations
So the boundary is:
Gate-level netlist  →  Physical floorplan
           ↑
     Synthesis ends here
________________________________________
 Where Timing Is Checked?
 
Timing is checked at multiple stages using:
OpenSTA

🔹 Timing Checks Occur:

1.	After synthesis
o	Ensures logic meets constraints
2.	After placement
o	Checks pre-route timing with estimated wire delays
3.	After CTS
o	Verifies clock skew and insertion delay
4.	After routing (signoff timing)
o	Final setup/hold verification with real parasitics
Timing analysis includes:
•	Setup checks
•	Hold checks
•	Slack calculation
•	Critical path reporting
So timing verification is continuous — not a single-step process.
________________________________________
5️⃣ Where GDS Is Produced
The final layout database (GDSII) is produced at the end of the physical design flow by:
OpenROAD
After:
•	Placement
•	CTS
•	Global routing
•	Detailed routing
•	Parasitic extraction
•	Final timing checks
The tool exports:
design.gds
This GDS file:
•	Contains all metal layers
•	Via definitions
•	Standard cell geometries
•	Ready for fabrication
The layout can then be viewed in:
KLayout
________________________________________

🏗️ Complete Flow Summary (Conceptual View)
RTL (Verilog)
   ↓
Yosys  →  Gate-level Netlist
   ↓
OpenROAD
   ├─ Floorplan
   ├─ Placement
   ├─ CTS
   ├─ Routing
   ├─ Parasitic Extraction
   ├─ Timing (OpenSTA)
   ↓
Final GDSII
   ↓
KLayout (Visualization)

# PHASE 3 — Local Installation (Self-Owned Environment)

## Requirements:

Use Oracle VirtualBox + Ubuntu 22.04 ISO.

## Task 3.1 — Install ORFS Locally && Task 3.2 — Install Official OpenROAD

Following dependencies are installed before clonit vsd-scl180-orfs:

```bash
sudo apt-get update
sudo apt-get install -y --no-install-recommends \
    wget curl unzip git git-lfs tcl-tclreadline make python3 python3-pip python3-numpy python3-pandas python3-matplotlib python3-jinja2 python3-xlsxwriter \
    xvfb xfce4 xfce4-session xfce4-panel locales xfdesktop4 xfwm4 xfce4-terminal thunar dbus-x11 x11vnc novnc websockify supervisor \
    magic ngspice gtkwave verilator iverilog \
    build-essential autoconf automake libtool m4 cmake ninja-build clang \
    tcl tcl-dev tcllib tcl-trf tclx tk tk-dev locales \
    perl libfile-which-perl ca-certificates gnupg lsb-release pkg-config \
    libcairo2-dev libgl1-mesa-dev libglu1-mesa-dev freeglut3-dev \
    libx11-dev libxext-dev libxrender-dev libxpm-dev libxaw7-dev libfontconfig1-dev \
    libreadline-dev libncurses-dev flex bison gawk tcsh time bc vim gedit \
    libqt5charts5 libyaml-cpp0.7
```
Now cloning of vsd-scl180-orfs is done

```bash
# Clone the ORFS repository
git clone https://github.com/vsdip/vsd-scl180-orfs
cd vsd-scl180-orfs
```

Now we build yosys usinf following command:

```bash
# Build Yosys
# Clone the repository
cd vsd-scl180-orfs/orfs/tools
git clone https://github.com/YosysHQ/yosys.git
cd yosys
# Entire path is cd vsd-scl180-orfs/orfs/tools/yosys
make config-gcc
make -j$(nproc)
sudo make install
```
```bash
# Building OpenROAD
cd vsd-scl180-orfs/orfs/tools/OpenROAD
# cloning OpenROAD repository
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD.git
cd OpenROAD
# Actual path is now  cd vsd-scl180-orfs/orfs/tools/OpenROAD
# Creating build directory
mkdir build
cd build
# Configure with CMake
cmake .. \
    -DCMAKE_BUILD_TYPE=RELEASE \
    -DTCL_LIBRARY=/usr/lib/x86_64-linux-gnu/libtcl.so \
    -DTCL_HEADER=/usr/include/tcl.h \
    -DCMAKE_INSTALL_PREFIX=/usr/local


   make -j$(nproc)
   sudo make install
```
Once OpenROAD and yosys is builded properly, Lets check the version

 ```bash
openroad -version
26Q1-1736-g93742072ba

yosys -V
Yosys 0.63+87.

make --version
GNU Make 4.3
```
# PHASE 4 — Re-Run RTL-to-GDS Locally

Once yosys and OpenROAD is installed we need to set their path in file "synth.sh" available in script folder.

```bash
export YOSYS_EXE = $(which yosys)
export OPENROAD_EXE=$(which openroad)
```

once path is set we will go to location 
```
cd vsd-scl180-orfs/orfs/flow
```
and change pdk to sky130hd.

Then we started to run  commands
## Synthesis:
 ```bash
make synth
# To view Gui
make gui_synth
```
## Screenshot:

![Synth](https://github.com/user-attachments/assets/d011dbe6-4622-4b77-9881-4c811182b025)

![Synth_1](https://github.com/user-attachments/assets/ab75c634-0b48-4e3f-8114-20a2364ee606)

![Synth_gui_command](https://github.com/user-attachments/assets/42a59874-f5e1-4cdf-9905-0ca1ef330fb5)

![Sunth_gui](https://github.com/user-attachments/assets/694d6d12-2e55-4296-a768-d2f0dc920bbe)

## Floorplan:

```bash
make floorplan
# To view Gui
make gui_floorplan
```
## Screenshot:

![Floorpaln](https://github.com/user-attachments/assets/b530ce88-e835-47e6-b501-e6ab58f5203b)

![florrpal_gui_command](https://github.com/user-attachments/assets/dbe99061-9be3-472e-8b11-2109846896f9)

![Floorpal_gui](https://github.com/user-attachments/assets/19767373-f250-4554-9b23-ce5024d2940b)



## Placement:

```bash
make place
# To view Gui
make gui_place
```
We encountered some error due to tight placement. So we reduced placement density from 0.2 to 0.1 and utilisation factor from 45 to 40.

## Screenshot:

![place](https://github.com/user-attachments/assets/a70ce6d2-8244-41cc-b2c4-216b2c16b929)

![pace1](https://github.com/user-attachments/assets/b0008ecf-cded-4200-b1b8-6041538e0161)

![palce_gui](https://github.com/user-attachments/assets/2df4af3c-5123-41c3-bb6b-301b0975eccc)

![palce_gui2](https://github.com/user-attachments/assets/f0742f75-8abb-4190-b022-198e74c6d7f4)

## CTS:

```bash
make cts
# To view Gui
make gui_cts
```
## Screenshot:

![cts](https://github.com/user-attachments/assets/72f404e9-e4f6-4e48-b64a-9f8f12463093)

![cts2](https://github.com/user-attachments/assets/038e65f0-8559-4e87-85bb-7db874dc8a49)

![cts_gui command](https://github.com/user-attachments/assets/862dccb4-71ba-4dd1-999f-8f88871a184c)

![cts_gui](https://github.com/user-attachments/assets/82e0a833-3a6d-42c7-8b52-332e798e297c)

![cts_gui1](https://github.com/user-attachments/assets/d7e6ab91-6a0e-44ba-9fe6-8a96f34a13d2)

## Route:

```bash
make route
# To view Gui
make gui_route
```
## Screenshot:

![route](https://github.com/user-attachments/assets/d0ee1c21-21d5-4d24-a47c-fc8c78bc00d9)

![route2](https://github.com/user-attachments/assets/50bb20ef-5ca1-4720-ac9a-f5eadae270e9)

![route_gui command](https://github.com/user-attachments/assets/fb66b02e-ecf4-4372-b56f-bbd98ed00930)

![route_gui](https://github.com/user-attachments/assets/b1393f30-55b5-4597-b7ac-ada5d2b3de32)

![route_gui1](https://github.com/user-attachments/assets/4a402090-5cb3-468a-a12b-cf69046354fe)

![route_gui 2](https://github.com/user-attachments/assets/7bde32b5-f000-4bda-aa6f-4b1116ce119c)

To generate final report/gds file, we use following command:

```bash
make final
# To view Gui
make gui_6_merged.gds
```
## Screenshot:

![final](https://github.com/user-attachments/assets/d0778dbe-d946-4cae-9fbb-b431bf86f4ad)

![final1](https://github.com/user-attachments/assets/81c3ae20-f76b-46c0-b7e5-8472f17a3e0f)

![final command](https://github.com/user-attachments/assets/c73c0370-1e97-477a-a16e-9303c1013a5c)

![make_final](https://github.com/user-attachments/assets/9df56ee2-22c5-4ace-acb1-618056213c94)

![make_final1](https://github.com/user-attachments/assets/369f8294-fcb7-491d-8c8b-78b82a244d9c)


# PHASE 5 — Debugging and Unix Literacy

●	pwd: Gives present working directory

●	ls: lists files

●	cd:moves between folders

●	grep: The grep command in Linux is used to search for specific text patterns inside files or command outputs. It is very useful for filtering logs, reports, or large files.

●	find: Used to find a file by name.

●	cat: View the contents of a file

●	less:The less command in Linux is used to view the contents of a file one screen at a time. It is very useful for reading large files like logs and reports.

●	echo:The echo command in Linux is used to display text or the value of variables on the terminal.

●	export: The export command in Linux is used to set environment variables and make them available to other programs or child processes.

## Screenshot:

![commands](https://github.com/user-attachments/assets/ccb65d4a-214e-45b6-a1e0-286fb263958b)

![comm1](https://github.com/user-attachments/assets/6cbd5d00-1568-4a0a-9930-540609f94e85)

![comm2](https://github.com/user-attachments/assets/54bda0bc-34bd-4653-bea7-55c24333267e)

![comm](https://github.com/user-attachments/assets/72b45400-79d5-404d-b19c-98904c69a61c)

![comm2](https://github.com/user-attachments/assets/65ca9765-2335-4a04-ad49-eb8a75eb69ee)














































































































