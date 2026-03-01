# VSDSquadron-RTL2GDS-SoC-Implementation
> 6 Weeks RTL2GDS SoC Implementation programme by VSD which focuses on System &amp; RTL Foundations to Physical Design and sign-Off using OPENLANE

![Static Badge](https://img.shields.io/badge/OS-linux-orange)
![Static Badge](https://img.shields.io/badge/EDA%20Tools-OpenLANE--Flow%2C_Yosys%2C_abc%2C_OpenROAD%2C_TritonRoute%2C_OpenSTA%2C_magic%2C_netgen%2C_GUNA-navy)
![Static Badge](https://img.shields.io/badge/languages-verilog%2C_bash%2C_TCL-crimson)



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

## Phase 1 — OpenLANE Flow Familiarity (RTL → Synthesis literacy) 

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

# Invoking PDN:

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













































