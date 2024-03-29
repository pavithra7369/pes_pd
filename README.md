# pes_pd

 <summary>TABLE OF CONTENTS </summary>

# Day 1
Inception of open-source EDA, OpenLANE and Sky130 PDK

+ How to talk to computers
  
  + Introduction to QFN-48 Package, chip, pads, core, die and IPs
  + Introduction to RISC-V
  + From Software Applications to Hardware
    
+ SoC design and OpenLANE
  
   + Introduction to all components of open-source digital asic design
   + Simplified RTL2GDS flow
   + Introduction to OpenLANE and Strive chipsets
   + Introduction to OpenLANE detailed ASIC design flow
     
+ Get familiar to open-source EDA tools
  
   + OpenLANE Directory structure in detail
   + Design Preparation Step
   + Review files after design prep and run synthesis
   + openLANE Project Git link description
   + Steps to characterize synthesis result

# DAY 2
Good floorplan vs bad floorplan and introduction to library cells

+ Chip floorplaning considerations
  
    + Utilization factor and aspect ratio
    + Concept of pre-planned cells
    + De-coupling capacitors
    + Power planning
    + Pin placement and logical cell placement blockage
    + Steps to run floorplan using openLANE
    + Review floorplan files and steps to view floorplan
    + Review floorplan layout in Magic
      
+ Library binding and placement
  
    + Netlist binding and initial place design
    + Optimize placement using estimated wire-length and capacitance
    + Final placement optimization
    + Need for libraries and characterization
    + Congestion aware placement using RePlAce

+ Cell design and characterization flows
  
    + Inputs for cell design flow
    + Circuit design step
    + Layout design step
    + Typical characterization flow
   
+ General timing characterization parameters
  
    + Timing threshold definitions
    + Propagation delay and transition time
   
# DAY-3
 Design library cell using Magic Layout and ngspice characterization

  + Labs for CMOS inverter ngspice simulations
    
     + placer revision
     + SPICE deck creation for CMOS inverter
     + SPICE simulation lab for CMOS inverter
     + Switching Threshold Vm
     + Static and dynamic simulation of CMOS inverter
     + Lab steps to git clone vsdstdcelldesign
     
+ Inception of Layout A CMOS fabrication process
  
     + Create Active regions
     + Formation of N-well and P-well
     + Formation of gate terminal
     + Lightly doped drain (LDD) formation
     + Source A drain formation
     + Local interconnect formation
     + Higher level metal formation
     + Lab introduction to Sky130 basic layers layout and LEF using inverter
     + Lab steps to create std cell layout and extract spice netlist
      
+ Sky130 Tech File Labs
     
    + Lab steps to create final SPICE deck using Sky130 tech
    + Lab steps to characterize inverter using sky130 model files
    + Lab introduction to Magic tool options and DRC rules
    + Lab introduction to Sky130 pdk's and steps to download labs
    + Lab introduction to Magic and steps to load Sky130 tech-rules
    + Lab exercise to fix poly.9 error in Sky130 tech-file
    + Lab exercise to implement poly resistor spacing to diff and tap
    + Lab challenge exercise to describe DRC error as geometrical construct
    + Lab challenge to find missing or incorrect rules and fix them

# DAY-4
 Pre-layout timing analysis and importance of good clock tree

+ Timing modelling using delay tables
  
    + Lab steps to convert grid info to track info
    + Lab steps to convert magic layout to std cell LEF
    + Introduction to timing libs and steps to include new cell in synthesis
    + Introduction to delay tables
    + Delay table usage Part 1
    + Delay table usage Part 2
    + Lab steps to configure synthesis settings to fix slack and include vsdinv
      
+ Timing analysis with ideal clocks using openSTA
  
    + Setup timing analysis and introduction to flip-flop setup time
    + Introduction to clock jitter and uncertainty
    + Lab steps to configure OpenSTA for post-synth timing analysis
    + Lab steps to optimize synthesis to reduce setup violations
    + Lab steps to do basic timing ECO

+ Clock tree synthesis TritonCTS and signal integrity
      
    + Clock tree routing and buffering using H-Tree algorithm
    + Crosstalk and clock net shielding
    + Lab steps to run CTS using TritonCTS
    + Lab steps to verify CTS runs

 + Timing analysis with real clocks using openSTA
   
    + Setup timing analysis using real clocks
    + Hold timing analysis using real clocks
    + Lab steps to analyze timing with real clocks using OpenSTA
    + Lab steps to execute OpenSTA with right timing libraries and CTS assignment
    + Lab steps to observe impact of bigger CTS buffers on setup and hold timing

# DAY-5
 Final steps for RTL2GDS using tritonRoute and openSTA

+ Routing and design rule check (DRC)
  
    + Introduction to Maze Routing A� LeeA�s algorithm
    + LeeA�s Algorithm conclusion
    + Design Rule Check
      
+ Power Distribution Network and routing
  
    + Lab steps to build power distribution network
    + Lab steps from power straps to std cell power
    + Basics of global and detail routing and configure TritonRoute
      
+ TritonRoute Features

    + TritonRoute feature 1 - Honors pre-processed route guides
    + TritonRoute Feature2 & 3 - Inter-guide connectivity and intra- & inter-layer routing
    + TritonRoute method to handle connectivity
    + Routing topology algorithm and final files list post-route

 

</details><details>
<summary>DAY 1  Inception of open-source EDA, OpenLANE and Sky130 PDK:
 How to talk to computers </summary>
 

Introduction to QFN-48 package,Chips,Pads,Core,Die,and IP's

Here we are taking aurdino board, we are discussing about the circled chip in the below picture
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/eb1d857d-55ba-43ca-9dff-0ac52a8c3ff4)

> Block diagram of aurdino board
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/cea47917-f860-4539-ba2a-fd8b8273ce09)

>  Te chip design of package QFN-48(Quad flat No-leads) is shown below
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/75264392-6050-4707-8999-2ab890f94965)

> Wire bounds are used to connect the pins to the boundaries of chip,this is how transfer signals from outside world enter into the interior of chip.

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/667b709c-0df0-4346-bf56-a8f8e004e14b)

> Pads -> We can send signals from pins through pads(signals enter and leave the chip through pads)

> core -> Core of chip is where digital logic placed

> Die -> A die is a small block of semiconducting material on which a given functional circuit is fabricated.

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/8797d493-9a38-4ded-ad16-867492954c9a)

In this course we are dealing with RISC-V SOC

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/7be844a6-4669-49cc-a547-ea270174b0af)

+ DAC,PLL,SRAM,ADC combined form *Foundry IP's*
+ Foundry is a place where chips get manufactured
+ IP's stands for Intelluctual property ,they are called intelluctual,since some amount of intellligence is required.
+ Foundry is the one we need to communicate with,to communicate with foundry there are soome interface files which foundry gives to us.

Introduction to RISC 

>  Implement RICS-V Specifications using some RTL,RTL used here is picorv32 cpu core and then from RTL to layout standard RTL-GDS flow.
> >RISC-V architecture->RTL->Layout

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/ed42f718-c358-44e4-8cb1-e72528ee48a4)


 From Software Applications to Hardware
How does different apps run on the chip?
+ The application software enters the system software, the system software contains OS,Compiler & Assembler.
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/72d02642-833c-4dff-a5d9-497fe4e68bd9)

+ The operating system(OS) performs the following functions :-Handle IO operation, Allocates memory, low level syetem functions
+ The compiler converts the c,c++ to *.exe file, the syntax off instructions depends on what kind of hardwaree is used(ex:-RISC V ,ARM)
+ The assembler takes the instructions and converts to machine level language.
+ the instructions after the compiler acts as an "Abstract interface" between the C language and the hardware, this Abstract interface is called as the *Instruction set architecture* or *Architecture of the computer*

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/9c902fd3-8923-47d3-977c-60e4fb2baab4)

  + The output of Assembler,is binary,we need to implement this in hardware,that is where hadware description language and the RTL is getting synthesized 
    into netlist,and from this Synthesized Netlist to hardware is Physical design implementation of the Netlist. 
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/6da1b319-be71-44c1-b37a-f71b14571a56)

</details> <details>
 
<summary> SoC design and OpenLANE </summary>

+ Introduction to all components of open source digital aasic design
>  Application specific integrated circuit requires RTL designs,EDA tools and PDK data.

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/db56038a-ec73-4da6-84fc-f8f4123dde72)

 + Examples of RTL designs available are librecores.org,opencores.org,github.com
 + Examples of some EDA tools available are openROAD,Qflow,spice,openLANE etc.

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/c07c89e0-d530-4f53-9320-2a386dfb0a6c)

+ PDK's (process design kit)
  A process design kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design 
 an integrated circuit. The PDK is created by the foundry defining a certain technology variation for their processes,
 PDK'S is collection of files for EDA tools used to design on IC(integraed circuits)
 PDK's acts as interface between FAB and designers.
 The first open source PDK is skywater,it was released in june 30 2020.

+ ASIC Design flow-> objective of ASIC flow is to take design from register tranfer level(RTL) all the way down to GDSII(format used for final layout)

+ Simplified RTL to GDSII flow
  
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/ca2b811c-7c68-4706-a7a5-b5a6f61732e2)
  
  1) Synthesis
    + Design is illustrated in circuits made out of components in standard cell libraries
      ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/cc493485-09ef-48af-9434-da53138fcb74)

    + Result is circuit described in HDL and usually referred to as gate level netlist.
    + each cell has different views/models
        + electrical ,HDL,SPICE
        + Layout (abstract and detailed)

  2) Floor and power planning
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/5e32f4ee-eb7c-4711-a3fe-44353a0ed2b6)

  3) Placement
     + Usually placement is done in two steps-> global,detailed
     + Global placements tries to find optimal postions for cells such positions are not nessecerily legal so cells may overlap or Go off Rows
     + In deltailed placement positions are alterred
       ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/64b875cd-5f57-485f-a8d6-4b8583daa872)

  4) Clock tree synthesis(CTS)
    +CTS delivers clock to all the sequential elements,within a minimum clock skew and in good shape(usually H OR X)
   ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/06738668-9b95-41e8-afe1-d7fcc02a95cb)

   5) Routing
      ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/50f6bf17-0170-477a-8349-36439e3bcc05)

     + Implement the interconnect using available metal layers
     + metal tracks form a huge grid
     + divide and conquer
        + global routing-> Generates routing guides
        + detailed routing->Uses of routing guides to implement the actual wiring

  6) Sign off
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/d6d20977-8cac-4c50-8fdc-d6eafbeae9b9)
     
     + Physical verification and Timing verifaction
     + physical verification includes DRC(Design rule checking),layout vsschematc(LVS)
     + Timing verification includes static timing analysis(STA0)


## Intoduction to openLANE and Strives chipsets
  + What is openLANE?
   OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen and custom methodology scripts for 
   design exploration and optimization.
  + strive is a family of open everything SOC's->open EDA,PDK's,RTL
    ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/ef4f24db-0820-45d3-aaf3-caa547d9c3ee)

  + Main goal is to produce GDSII with no human intervention (no-human-in-the-loop),no LVS violations, no DRC violation.
  + OpenLANE can be used to harden macros and chips.
  + openLANE has 2 modes of operation->autonomous or interactive
  + openLANE has design space exploration
  + openLANE comes with large number of design examples,there are 43 with best configurations

 ## Introduction to OpenLANE detailed ASIC design flow
 ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/76821ce9-9c34-4b43-a807-4c23cd9e18fc)
   + Starts at design RTL ends at final layout in GDSII
   + Synth Exploration utility can used to generate a report that shows about the design delay and area is effected by the Synthesis Strategy and based on 
     this exploration we can pick the best strategy
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/37c98d05-11cd-43b5-869b-7d721edeb3a1)

   + Design exploration is used to switch design configurations for findinng the best configurations
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/53636773-7040-4ac2-a625-aaaff452d49c)

   + The design Exploration is also used for Regression testing(CI)
      ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/9259922e-4869-4c27-bca4-5c0d294785ab)
   + After regression testing, DFT(design for test) is performed
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/c3d348d0-7e49-4c74-bbb8-0e69850d56fc)
   + Physical implementation-> It is also called automated PnR(place and route)
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/4a6f12bb-61f1-4f92-aff1-d66509504b16)
   + After Physical implementation we Do LEC(Logic Equivalence Check) using Yosys.
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/7ea6f90d-b963-4b1f-aab7-2b4b45403618)
   + The next step is Fake antenna diodes insertion or dealing with antenna rules violations
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/ad794448-a510-490d-890a-6c7f9e547b3e)
     There are 2 solutions:-
     1)Briding->attaches a higher layer immediately , requires router awarness
     2)Add antenna diode cell to leak away charges , antenna diodess are provided by SCL
     There is a preventive approach , it is to add fake antenna diode next to every cell input after placement, run the antenna checker(magic),if the checker reports a violations on the cell input pin,replace the fake diode cell by a real one.
     
   + After this process we have the SIGN OFF which has the Static timing analysis,design rule checking and Layout vs schematic.
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/7ff10e21-613c-412d-b260-4ad0e55d7324)
   + Physical verification DRC & LVS
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/87d7eb75-aad0-449b-90d5-003b93e70d0d)

</details> <details>
<summary>
 Get familiar to Open-source EDA tools
</summary>
 
## OpenLANE Directory structure in detail
> openLANE is actually a flow that contains several EDA tools.
> cd->change directory
> ls->listing
> ltr->order
> la---help ->to know about a command
+ The PDK varient used here is skywater130nm
+ skywater130nm has two subdirectories
     + libs.ref (process specific)
     + libs.tech (specific to tool)
+ fd-> foundry name
+ sc->standard cell
+ hd->high density
  
  ![WhatsApp Image 2023-09-16 at 08 18 03](https://github.com/pavithra7369/pes_pd/assets/143084423/7e07ef89-7d10-48a0-a42b-52790e880cf6)
  
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/fc807a12-3a37-49a3-84ef-6422049c16db)

## Design Preparation Step

+ Go to terminal
  
  > cd/desktop/works/tools/openlane_workshop__dir/openlane
  
  > docker
  
  > ./flow.tcl -interactive
  
  > package require openlane 0.9
  

![WhatsApp Image 2023-09-16 at 07 46 40](https://github.com/pavithra7369/pes_pd/assets/143084423/69cd4647-e7ca-4550-ab4d-745b420c16be)
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/d73bb692-c00b-4352-9d28-c41d25d1eb51)
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/a0af3f22-ef6b-40fe-841a-93436622167e)

## Review files after design and run synthesis

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/97856699-ff57-49b7-b55c-4f70e39670ae)
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/84c5f0c6-1fd9-4c5a-b44c-c2505007757b)

To calculate flop ratio
![WhatsApp Image 2023-09-16 at 08 49 39](https://github.com/pavithra7369/pes_pd/assets/143084423/b8d3a8a0-65be-4cca-a547-02cdc932ec83)
![WhatsApp Image 2023-09-16 at 08 49 50](https://github.com/pavithra7369/pes_pd/assets/143084423/578b7c4d-adb0-4395-9904-6dd2317b1f6b)
+ Divide 1634 by 17323
+ 17323 is the Number of cells
+ (1634/17323) ->0.094 This is the flop ratio 

 
 </details><details>
<summary> DAY 2 Good floorplan vs bad floorplan and introduction to library cells: Chip Floor planning considerations </summary>
  
  ## Utilization factor and aspect ratio
    
   + Define Width and height of core and die
     
 ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/8262b5ed-03e8-430b-89ce-6fdd6afc0a9a)
 > Dimensions of chip depend on dimensions of logic gates and flip flops
 > Convert the symbols into physical dimension
 > A core is the section of the chip where the fundamental logic of design is pplaced
 > Die is a small semiconductor material speciment on which the fundamental circuit is fabricated.
 > Place all the logical cells inside the core, in this case the logical cells occupies the complete areea of core, so, 100% utilization

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/d090f55b-7c7f-4487-829c-b40b07bcd14e)
> When utilization factor is 1, means that the core is completely occupied and we cannot add any more cells in the logic
> If utilization factor is not equal to "1" then we can add additional cells
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/880ead6f-e749-4fab-b821-9836895f5c67)
> Aspect ratio is 1 for a sqaure chip,and for a rectangular hcip aspect tatio can be in decimal value.

  + Define locations of preplaced cells
    ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/6b28ea4e-068b-44ac-855f-d23a703ced3f)
> In preplaced cells we can divide the logic gates into two blocks
> Both the blocks arre impplemented separately,by extending IO pins and then black box the blocks
> separate the black bosex as 2 different modules
> Advantage of this process is , if part of logic is used multiple times on a network,we need not implement multipple times, we can just black box 
 them,now these blocks can be connected multiple times into the netlist and can be used whenever required

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/bfa748c5-7031-4afe-80e7-dec6a4b8d6f1)

> These IP's/blocks have user-defined locations and hence are placed in chip before automated placement-and-routing and are called pre placed cells.
> The llocaations of pre placed cells are not touched when we go on with the design cycle, so once the pre-placed cells are placed their locations can't 
 be moved in the completely design cycle,so locations of pre placed cells are should be very well defined
> Piece of macros that is used multiple times (we can implement memories once, then they are reused multiple times,so we need not implement in each and 
 every time)
  
   + De-coupling capacitors
     > we use decoupling capacitors,they are huge capacitors completely dilled with charge, when there is switching aactivity,decoupling  capacitors loses 
      some charge
     > It's decoupled from the main circuit
       ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/2cfa7676-e3bb-4338-b5cf-4f26ac140f64)
     > This block will not miss any switching activity and cross talk


  + Power planning
    > Power planning during the Floorplanning phase is essential to lower noise in digital circuits attributed to voltage droop and ground bounce.
    > When a transition occurs on a net, charge associated with coupling capacitors may be dumped to ground. If there are not enough ground taps charge 
      will accumulate at the tap and the ground line will act like a large resistor, raising the ground voltage and lowering our noise margin.
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/19b83ea7-1d76-4ee6-b03e-71e5aa0535db)

 + Pin Placement
     > After power planning the next step is pin placement, Input and Output pins are provided
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/40e84154-703b-4ed5-ba78-227b49559ea9)

+ Logical cell placement blockage
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/ae193a2c-09a1-4191-a469-a03f2308f1d5)
  > Now floorplan is ready for placement and routing step

  ## Steps to run FLoorplan using OpenLANE
  After synnthesis, open terminal and run floorplan
  > run_floorplan
  ![WhatsApp Image 2023-09-18 at 23 10 30](https://github.com/pavithra7369/pes_pd/assets/143084423/0381eaf2-5018-403d-b579-8f293bffddb0)

  > magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def 
  ![WhatsApp Image 2023-09-18 at 23 19 32](https://github.com/pavithra7369/pes_pd/assets/143084423/555eac51-e527-4452-95e9-c8f91e7d3278)
  
  ![WhatsApp Image 2023-09-18 at 23 15 30](https://github.com/pavithra7369/pes_pd/assets/143084423/50ef414f-1756-48e3-bccb-b59bb224208f)

  ![WhatsApp Image 2023-09-18 at 23 20 31](https://github.com/pavithra7369/pes_pd/assets/143084423/f1f03ca9-0d1e-4866-8742-5a5f7b3bc6ff)
  > To know details of a cell gtype 'what' in tkcon window
  ![WhatsApp Image 2023-09-18 at 23 21 35](https://github.com/pavithra7369/pes_pd/assets/143084423/fee560b8-e5ad-412f-be04-fb9f023e09a5)

 </details><details>
  <summary>Library Binding and Placement </summary>

 +  Placement and routing
    1) bind netlist withphysical cells
     > shape of the gates represent the functionality of the gates, inreality all gthe gates are represented as boxes
     > each components are given proper shape
     > library has all the  height,width,delay informations of a particular cell and the required conditions of the cell,libraries can be further divided 
      by shape/size and delay information.
     > Libraries also contain various flavour of a particular cell
   2) Placement
     > ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/e73abb22-8e4d-425e-b4ac-9595773c60f7)
     > Pre placed cells are not to be affected and no cells should be placed in that area.
     > In the above picture we see that the FF1 and FF2 are pplaced accordingly, this causes huge length
  3) Optimize placement
     >  here we will estimate the capacitances, and wire length based on insert repearters
        ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/6401b75f-6a2b-405e-b6a5-e07afdf62e49)
     > We have to maintain signal integrity, so we use repeators,which are buffers that will re-condition the  original signals make a new signal and 
      replicates the original signal.
     > There is loss of area despite maintainng signal integrity
     > Signal integrity needs to be maintained in all the cells
     > The distance between each cell is calculated by slew/ transition
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/d5080495-b715-42de-92de-718afb6df1d2)
     > Logic is optimized based on placement conditions
 4) STA (static timing analysis)
     > Static Timing Analysis can be done only for Register-Transfer-Logic (RTL) designs. Functionality of the design must be cleared before the design is subjected to STA.
      
+  Library characterization and modelling
  Libraries provide standardized building blocks that enhance design productivity and reusability, while characterization provides the essential data 
  needed to accurately model and simulate the behavior of these components, ensuring that the final design meets its performance, power, and reliability 
  goals.

+ Congestion aware placement using RePlAce
   > Placement is of 2 types: detailed placement and global placement
   ![WhatsApp Image 2023-09-16 at 14 46 07](https://github.com/pavithra7369/pes_pd/assets/143084423/6363b009-3d30-46a2-a3ad-e9214e838f55)
   ![WhatsApp Image 2023-09-16 at 14 47 23](https://github.com/pavithra7369/pes_pd/assets/143084423/bd7f5dea-5380-45df-9beb-91a6509d32fd)
  > Placement of standard cells is shown below
   ![WhatsApp Image 2023-09-18 at 23 31 19](https://github.com/pavithra7369/pes_pd/assets/143084423/591fef3e-a9e3-4b7d-916d-f0fe107502d9)

 </details><details>
 <summary> Cell design and characterization flows
 </summary>
  
  ##  Inputs for cell design flow
   > Standard cells are placed in library
   > one of the section stores all the stanadard cells
   > Library consists different cells of functionalaity, Vt ,size
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/5f89ad19-bcee-4fd9-895d-02fd4496800d)

   > for example the IC design flow for inverters, is represented in the form of shape,timinng behaviour, power charcteristic and so on..
   > IC  design flow consists of inputs, design steps and outputs
   > Further inputs have PDK's
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/65c77a4b-2825-4fa3-a7ab-774a70e82dc8)

  ## Circuit design steps
  > Here in the first step user design specifications are provided such as cell height,supply vltage,metal layer,pin location and so on.
  > In the design steps circuit design,layout design and characertization is present
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/f0d57c35-1f87-489c-af1a-15428f1002e8)
  > In circuit design circuit is designed with user specifications
## Layout design steps
 > In layout specification we design the logic using PMOS and NMOS and then find the 
    Euler's path for both nmos network and pmos network
 > Based on Eulers path we draw sstick diagram.
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/85140afc-2282-4261-a356-5935a7b42622)
 > The output of layout design will be GDSII, LEF(defines width and height of cell),extracted spice netlist (provides information of parasatic 
   capacitances and resistances)
>Characterization provides Timing,noise and power information

## Typical characterization flow
+ The characterization flow is as follows:-
  
 > Read in the model
 > Read the brhaviour of buffer
 > Recognize the behaviour of buffer
 > Read subcircuits of inveter
 > Attach necessary power source
 > Apply stimmulus
 > Provide necessary output capacitances
 > Provide necessary simulation command

+ The characterization flow is made into a configuration file to the characterization software called 'GUNA'
+ GUNA generates timing, noise, timing,power
+ Characterization is divided into
   > Timing characterization
   > Power characterization
   > Noise characterization 

</details> <details>
<summary> General timing characterization parameters</summary>

 ## General timing characterization parameters
 ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/90bad427-7ed1-450f-8ed7-d70abfcabd04)

## Propagation Delay and Transition time

+ Propogation delay -> Propagation delay of a gate or cell is the time it takes for a signal at the input pin to affect the output signal at output pin. 
 For any gate propagation delay is measured between 50% of input transition to the corresponding 50% of output transition.
   > Propagation delay=time(out_fall_thr)-time(in_fall_thr)
   > Propagation delay=time(out_rise_thr)-time(in_rise_thr)
   > Negative delay is not expected , negative dlay indicates poor choice of threshold points.
+ Transition time -> Transition delay or slew is defined as the time taken by signal to rise from 10 %( 20%) to the 90 %( 80%) of its maximum value.
  > Rise time transition  = time(slew_high_rise_thr) - time (slew_low_rise_thr)
  > Fall  time transition  = time(slew_high_fall_thr) - time (slew_low_fall_thr)



</details> <details>
<summary>DAY 3 Design library cell using Magic Layout and ngspice characterization: 
 Labs for CMOS inverter ngspice simulations</summary>

## Placer revision
> 4 Stratergies are supported by IO placer.

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/4456ad1f-49bf-4b36-9b44-892bcffdee0f)

> we can change the floorplan

## SPICE deck creation for CMOS inverter
 + To simulate standard cells we need to create spice deck for complete netlist
  1) component connectivity
  2) component values
  3) Identify nodes
  4) Name nodes

+ Spice deck is shown in below picture
     ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/c3ce06c5-435b-4960-9d65-8b1dbd2523fa)

 ## SPICE simulation lab for CMOS inverter
 ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/6787d554-16c5-4f1d-b0f3-2cff3d0c24fa)

## Switching Threshold
> CMOS is robust device
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/11104bbd-1569-4273-87ef-d4d80b40eac6)
+ switching threshold is a point wherre vin=vout
+ At the intersection both PMOS and NMOS are in saturation
  
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/01e39412-2e3a-42d9-a3af-ad6c4c09d930)

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/916d9dcb-e137-48ae-b276-fb25f60986a6)

## Static and dynamic simulation of CMOS inverter
> In dynamic simulation we provide pulse input and simulation is trasient analysis
 ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/6e2374de-5f25-468e-b931-32a1599a0caf)

> Rise delay -> 1.1667277-1.0146
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/de5904c9-1259-4e80-b7e8-922bfca93edc)

> fall delay -> 2.07653-2.00486 
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/8dd1c6f2-b172-41e8-a919-496927c2fe46)

## Lab steps to git clone vsdstdcelldesign
Use the below url to clone

> git glone https://github.com/nickson-jose/vsdstdcelldesign.git
Command
> magic -T sky130A.tech sky130_inv.mag &

> ![Screenshot 2023-09-16 173200](https://github.com/pavithra7369/pes_pd/assets/143084423/097eac3f-3a9e-47ba-b3d0-457fa228625e)

</details> <details>
<summary>Inception of Layout A CMOS fabrication process</summary>
 
## 16 Mask process
## Create Active regions
1) Selecting a substrate
   
   ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/e2205b8d-657c-4ae5-aa98-343bb1e60ead)

2) Create active region for transistors
 ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/af09f229-3476-478f-ac38-283511a141cb)
## Formation of N-well and P-well
3) N-well and P-well formation
   > N well is created for PMOS andd P well is created for NMOS
 ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/594ed881-1269-4d7f-8fe3-547e9446490b)
## Formation of gate terminal
4) Formation of gate
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/d590668e-9059-40e3-820a-ac27cbccdbbc)
## Lightly doped drain (LDD) formation
5)Lightly doped drain (LDD) formation
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/258cd0d0-590a-4c77-8f1e-4b1d4f12c89e)
## Source and Drain formation
6) Source and drain formation
   ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/675f3c09-3b76-403d-9a30-4ea762cde5a2)
## Local interconnect formation
7) Local interconnect formation
   ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/48234735-86a7-4c91-83b2-544a299dc164)
## Higher level metal formation
8) Higher level metal formation
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/d8462cd4-3569-431c-9ea7-f7ec5b1455c7)
 ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/9e48103f-f022-494d-b423-cc8adf7038c0)

## SKY_L8 - Lab introduction to Sky130 basic layers layout and LEF using inverter
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/8c04ebfa-dd14-4a2e-8f05-b8306865d3ed)
DRC Errors
> To check for errors, go to DRC and click on 'DRC Find next error'
> To know what the error is actually,it will be presnt in the tkcon window
 ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/e9446041-2205-4ff3-a848-2713c4321e8f)

## Lab steps to create std cell layout and extract spice netlist
> To extract spice command

   > extract all
   > ext2spice cthresh 0 rthresh 0

![WhatsApp Image 2023-09-16 at 19 26 27](https://github.com/pavithra7369/pes_pd/assets/143084423/75526019-b416-49f2-b4ff-3fd21fdbbf8c)
> we use pex file to create spice files
> vim sky130_fd_sc_hd
> edit the file
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/f61ae971-eec0-456f-86ab-8d1fc8a09bd1)


</details> <details>
<summary>Sky130 Tech File Labs</summary>

## Lab steps to create final SPICE deck using Sky130 tech
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/b4f48b58-2311-4e4f-9ac6-1d09d316b850)

> In the above image 'Y' represents drain, 'A' represents drain, 'VGND' represents source,'VPWR' represents substrate.
> Scaling -> dimension*scale

## Lab steps to characterize inverter using sky130 model files

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/d55d5876-9520-49ce-903b-5c771c959b5c)

> ngspice

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/a7ac530c-c1d7-4fea-aac6-624594e4fc2c)

> Plot
  > To plot we use the command
  > plot y vs time a
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/f32d30db-d0fb-461e-ac73-f8707a288407)


##  Lab introduction to Magic tool options and DRC rules

 Magic is a venerable VLSI layout tool, written in the 1980's at Berkeley by John Ousterhout, now famous primarily for writing the scripting interpreter 
 language Tcl. Due largely in part to its liberal Berkeley open-source license, magic has remained popular with universities and small companies. The 
 open- source license has allowed VLSI engineers with a bent toward programming to implement clever ideas and help magic stay abreast of fabrication 
 technology. However, it is the well thought-out core algorithms which lend to magic the greatest part of its popularity. Magic is widely cited as being 
 the easiest tool to use for circuit layout, even for people who ultimately rely on commercial tools for their product design flow.
 > http://opencircuitdesign.com/magic/

 ## Lab introduction to Sky130 pdk's and steps to download labs
 SKY130 pdk SKY130 is a mature 180nm-130nm hybrid technology developed by Cypress Semiconductor that has been used for many production parts. SKY130 is 
 now available as a foundry technology through SkyWater Technology Foundry.
 > wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
 > tar xfz drc_tests.tgz
 > cd drc_tests
 > > magic -d XR
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/23bf15d8-0cf2-4357-9e1c-2424c85f1734)
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/c92a648c-612e-4e55-9244-37f6b92f83e9)

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/4f7ef806-1785-4187-8a2c-7739751ac786)

![WhatsApp Image 2023-09-16 at 20 50 18](https://github.com/pavithra7369/pes_pd/assets/143084423/13822bc5-8521-4ac5-b7e0-a584601f110d)

 
+ To see DRC error select area and type drc why in tkcon
  
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/21cbc77b-ab16-440c-a551-450c5b35fbd4)

+ To fix the error open the sky130A.tech file using a editor and search for poly.9
  
+ Now load the sky130A.tech file again and type the command drc check
  
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/9f616ff5-e858-4767-b5fe-85d60bef53c1)

+ DRC error as geometrical construct
  
 > Open the nwell.mag file in magic. Seletch the nwell.6 and type the commands

   > cif ostyle drc
   > cif see dnwell_shrink
   > cif see dnwell_missing

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/fbe2e7fb-0f3e-4329-a70f-735f11548005)

+ Error
  
![image](https://github.com/pavithra7369/pes_pd/assets/143084423/16bb4f67-fd6a-4995-abf7-c1f1a5077691)

> fix the error

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/2530a6d9-578b-4253-bc0f-756a039dec96)

+ Now save the file and run DRC check

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/6d9a886e-d943-421b-89c0-9fd47dd77101)

</details><details>
<summary>DAY-4
Pre-layout timing analysis and importance of good clock tree</summary>
 
## Timing models using delay tables
+ LEF files contain information of input ports,output ports,power and ground port and so on..
+ Guidelines for PnR are:
  + Input and output ports must lie on the intersection of vertical and horizontal tracks
  + Width of the standard cell should be odd multiples of the track pitch and height should be odd multiple of vertical track pitch
  >  ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130fd_sc_hd/tracks.info

![WhatsApp Image 2023-09-17 at 20 02 21](https://github.com/pavithra7369/pes_pd/assets/143084423/45af9777-cd86-4e80-8c34-db9572263e47)

+ Pins must align with the li1 and met1 in preferred routing directions.

+ The Xspacing and Yspacing in the track file

![WhatsApp Image 2023-09-17 at 20 18 04](https://github.com/pavithra7369/pes_pd/assets/143084423/c5655f88-028c-46aa-b76f-9dc82b11771d)
+ Layer after setting the grid is in the below picture
![WhatsApp Image 2023-09-17 at 20 26 26](https://github.com/pavithra7369/pes_pd/assets/143084423/f5ee832b-a573-48f2-a8ba-ed78d7a83e01)

## Lab steps to Convert Magic Layout to Standard Cell LEF
> Now layout is done,port definitions are require for extracting LEF files
+ To define ports
    > Select the layer->Edit cell->Text
+ To Define the purpose of the port, for that we do port class and port use
    > select the port-> set the port class and port use here
+ Convert magic layout to standard cell LEF
  
1) In tkcon window of the 'sky130_inv.mag' file we the can change the name
   > save sky130_vsdinv.mag
2) To extract the LEF file type
   > lef write
3) type
   > less sky130_vsdinv.lef
   
  ![WhatsApp Image 2023-09-17 at 21 06 01](https://github.com/pavithra7369/pes_pd/assets/143084423/af68351c-d87b-4577-84f0-62850429d453)

5) Copy the .mag file that we created to the 'src' folder of picorv32a folder.
   ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/e325b645-feac-4a96-a7bf-7dfa5a5a0e2c)

+ Modify the contents
  ![WhatsApp Image 2023-09-19 at 09 29 19](https://github.com/pavithra7369/pes_pd/assets/143084423/84b74d16-307d-4b4f-b15c-09f1dd06133a)

+ now in openlane type the following commmands
  > prep -design picorv32a -tag 18-09_05-15 -overwrite
  > set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
  > add_lefs -src $lefs
  
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/bc538420-5650-4ecd-8f74-9661f05591a2)

  > run_synthesis
  ![WhatsApp Image 2023-09-19 at 09 33 48](https://github.com/pavithra7369/pes_pd/assets/143084423/809552f7-dbef-4b6b-a865-786a5f21e835)
  > run_floorplan
  > run_placement
  > magic -T 
  ![WhatsApp Image 2023-09-19 at 09 24 39](https://github.com/pavithra7369/pes_pd/assets/143084423/b2d14e91-7f8e-4990-8a82-e87745b506be)
  ![WhatsApp Image 2023-09-19 at 09 24 55](https://github.com/pavithra7369/pes_pd/assets/143084423/d6bc8511-5307-4551-8284-3d6e22d0a303)
  ![WhatsApp Image 2023-09-19 at 09 37 52](https://github.com/pavithra7369/pes_pd/assets/143084423/42390143-4039-42dc-8c4d-cd5ad246c35f)

## Delay tables
 A delay table lists the amount of delay as a function of one or more variables, such as input transition time and output load capacitance. From these table entries, the tool calculates each cell delay.
+ Delay tables purpose is represent the delays encountered by signals as they pass through various components of a digital circuit.
+ Components of Delay Tables are Input Conditions,Gate Delay,Interconnect Delay,Output Loads
</details><details>
<summary>Timing analysis with ideal clocks using openSTA
</summary>
## Setup timing analysis and introduction to flip-flop setup time
 + Setup time is the minimum amount of time the data signal should be held steady before the clock event so that the data are reliably sampled by the clock. This applies to synchronous circuits such as 
  the flip-flop.
+ The setup time (Ts) for a flip-flop determines when a data input signal must be stable before the arrival of the clock edge 

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/87abec98-59b4-47d7-83f4-fe89b52db872)


sky_Vsdinv
![WhatsApp Image 2023-09-23 at 07 27 00](https://github.com/pavithra7369/pes_pd/assets/143084423/6f79b01b-83f9-4dd2-997e-be32eb3425f6)


## Introduction to clock jitter and uncertainty
+ Clock Jitter-> Clock jitter is the variation of a clock signal's frequency or period. Either measurement carries the same information, but the period measurement is a simple time interval measurement easily performed using a real-time oscilloscope.
+ Clock Uncertainty-> The uncertainty of the clock is known as clock_uncertainty. For setup checks, you subtract the uncertainty from the clock period and add hold. If you were adding to the data path for setup, the resultant timing number is the same.

## Clock Tree Synthesis

command 
>run_cts
![WhatsApp Image 2023-09-19 at 09 47 20](https://github.com/pavithra7369/pes_pd/assets/143084423/b3905059-7cb0-47d8-ac1d-6ede1224c987)
![WhatsApp Image 2023-09-19 at 09 47 20](https://github.com/pavithra7369/pes_pd/assets/143084423/09a87a64-5d2e-4af1-89d7-df978d4d55b3)


## Clock Tree Synthesis TritonCTS and Signal Integrity
Post CTS- STA Analysis->OpenSTA is an open-source static timing analysis tool that is commonly used in digital circuit design.
In the openlane window type
> openroad

![image](https://github.com/pavithra7369/pes_pd/assets/143084423/3d672faf-9159-4a3c-8295-a48b494809a1)

</details><details>
<summary>Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA
  </summary>summary>
  Power Distribution Network and Routing
  > gen_pdn

  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/8d0f1ad6-5492-47ce-9594-b84c63fd027e)

  > run_routing


  ## SPEF Extraction
  SPEF is extracted after routing in Place and route stage. This helps in the accurate calculation of IR-drop analysis and other analysis after routing. This file contains the R and C parameters 
  depending on the placement of a tile/block and the routing among the placed cells.
  > cd Desktop/work/tools/SPEF_Extractor
  >python3 /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/16-09_19- 
 58/tmp/merged/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/18-09_19-58/results/routing/picorv32a.def
  > Spef extracted is created, path is given below
 > /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/18-09_06-26/results/routing/
