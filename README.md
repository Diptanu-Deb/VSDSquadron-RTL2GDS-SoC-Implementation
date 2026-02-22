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
 
<img width="991" height="676" alt="image" src="https://github.com/user-attachments/assets/bf1759e2-741a-43ac-9444-fdc09d063eb0" />

