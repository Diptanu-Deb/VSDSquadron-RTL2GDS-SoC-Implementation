# VSDSquadron-RTL2GDS-SoC-Implementation
6 Weeks RTL2GDS SoC Implementation programme by VSD which focuses on System &amp; RTL Foundations to Physical Design and sign-Off using OPENLANE




The curriculum of this workshop will be carried out in different phases and will be completed in 6 weeks.
In WEEK 1, we will be deep diving into Digital VLSI SoC Design and Planning, which is the foundation of this workshop.
Now Let’s begin with a simplified RTL2GDSII Flow which is given below:

<img width="999" height="304" alt="image" src="https://github.com/user-attachments/assets/d4d4853d-d09e-4123-aa95-e4294c988c51" />

 

Now what is PDK?

The main motto of our work is to make our design manufacturable. The place where designs are realised physically in silicon wafer is called Fab or fabrication Facility.

So, while designing any chip we need to consider some rules imposed by fabrication facility to make our chip physically realisable.

PDK stands for Process Design Kit, which act as an interface between designer and fabrication facility. It is a set of files and tools used in the semiconductor industry to model a foundry's manufacturing process inside an EDA tool.

In our entire design process, we will use the same required PDK.

Now moving to RTL2GSDII Flow,
Once we have the required RTL Code, the first step is to synthesize the RTL.
Synthesis: This process gives us the optimised gate level netlist.

Floor planning and power planning: In this step, we will define the aspect ratio of the core, Place the Macros and De-cap cells, carryout power planning, place pre-placement cells, define pin location.

Placement: In this stage we will place the standard cell in standard cell Row site and legalize their placement.

CTS: Clock tree synthesis is the stage where we will give clock signal to every block in a way that we have minimum skew and low latency.

Routing: Routing is making physical connections between signal pins using metal layers.

It can be divided into two steps:

a)	Global Routing: This step identifies the possible routing tracks.

b)	Detailed Routing: This step physically connects the signal pins.

Sign-Off: This steps checks whether there is any DRC violations or not, carry out schematic vs LVS check, and perform STA analysis.


The entire design will be done using OpenLane, which is a flow and comprised of many open-source EDA tools like Yosis+abc,magic,Ngspice etc.

The main goal is to have a clean GDSII without human intervention.

OpenLane ASIC flow diagram is given below:

 
<img width="1081" height="615" alt="image" src="https://github.com/user-attachments/assets/f11c5bef-0c0f-438d-bc2e-1dfde73e6681" />


In this Week all the labs will be carried out in GitHub Codespaces (vsd-openlane).

Phase 1 — OpenLANE Flow Familiarity (RTL → Synthesis literacy)

We will run “picorv32a” design synthesis using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform synthesis:

1)	Once noVNC Desktop is opened, go to terminal -> cd OpenLane
   
2)	Then type make mount (to invoke OpenLane flow)
   
3)	Type ./flow.tcl  -interactive (we can invoke the OpenLANE flow in the Interactive mode using this command)
   
4)	Then type package require openlane (required packages for proper functionality of the OpenLANE flow)
   
5)	prep -design picorv32a (As OpenLANE flow is ready to run any design and initially we have to prepare the design creating some necessary files and we run this command and our design name is picorv32a)
    
6)	run_synthesis (To synthesize the RTL code of the design)
    
7)	To get out of the OpenLane flow type exit.
    

Screenshots are attached below:

<img width="1004" height="596" alt="image" src="https://github.com/user-attachments/assets/3b1a5e32-cb00-41e3-a732-ace4e5628fef" />

<img width="940" height="641" alt="image" src="https://github.com/user-attachments/assets/200871bb-c7ca-451a-851c-21c539eb5cba" />

<img width="940" height="694" alt="image" src="https://github.com/user-attachments/assets/0431aeef-a4e7-43b4-b7c1-08079d1f489f" />

Phase 2 — Floorplan Fundamentals (macro awareness for Caravel blocks)

In this phase we will look into some Floorplan concepts and run floorplan in Openlane.

1)	Utilization factor and aspect ratio:

Aspect ratio: The ratio of the height of the chip core to its width.

<img width="551" height="175" alt="image" src="https://github.com/user-attachments/assets/72aca877-071f-4f47-8249-fa6a0e63ff4c" />

2) Utilisation factor: Ratio of  Area occupied by standard cell to the total core Area.

Concept of MACROS:

A macro is a large pre-designed functional block used in chip design that is treated as a single unit during placement and routing.

Unlike standard cells, macros are pre-designed, Pre-characterised, Pre-verified and larger in size.

Macros ae placed manually during floor planning.


A short explanation (your own words): why macros like RAM behave differently than standard cells:

RAM macros behave differently from standard cells because they are large, custom-designed memory blocks treated as fixed black boxes in the design flow, with timing and power dominated by internal memory array effects rather than simple gate delay.

Standard cells Can be resized (different drive strengths) whereas macros are of fixed size and cannot be resized.

During synthesis, standard cells can be optimised but macros cannot be optimised.

Screenshots are attached below:

Invoking floor plan command

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




Layout in  Magic

![Screenshot 2026-02-23 013235](https://github.com/user-attachments/assets/83fc78a3-0b93-4ad8-8aa5-0c209a4e2b29)

Pin Details:
![Screenshot 2026-02-23 013402](https://github.com/user-attachments/assets/b01223ee-dc3d-45f9-b980-d903280f71c5)

![Screenshot 2026-02-23 013439](https://github.com/user-attachments/assets/60a4ec04-b069-4176-bb51-269020d6a212)

![Screenshot 2026-02-23 013816](https://github.com/user-attachments/assets/0052f0da-c8a0-4bb8-b9c4-e4318ecd2f97)

![Screenshot 2026-02-23 012200](https://github.com/user-attachments/assets/f7b41943-bdda-4502-975d-bc829f05d54b)

Phase 3 — Timing Literacy with Ideal Clocks (OpenSTA + ECO mindset)

Timing checks like setup and Hold time checks are carried out to ensure that timing constraints are met.

Setup Time: It is the minimum amount of time required for the data signal to be remain stable before the arrival of active edge of the clock.

Hold Time: It is the minimum amount of time required for the data signal to be remain stable after the arrival of active edge of the clock.






















