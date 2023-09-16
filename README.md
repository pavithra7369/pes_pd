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

# DAY 1  Inception of open-source EDA, OpenLANE and Sky130 PDK

</details><details>
<summary> How to talk to computers </summary>
 

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
<summary>  DAY 2 Good floorplan vs bad floorplan and introduction to library cells </summary>

 </details><details>
<summary> Chip Floor planning considerations </summary>
  
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
  > Now floorplan is ready forr placement and routing step

  ## Steps to run FLoorplan using OpenLANE
  After synnthesis, open terminal and run floorplan
  > cd floorplan
  ![WhatsApp Image 2023-09-16 at 13 35 35](https://github.com/pavithra7369/pes_pd/assets/143084423/b93e0714-5bdd-414b-9b05-4f9ae317c04e)
  
  ![WhatsApp Image 2023-09-16 at 13 39 53](https://github.com/pavithra7369/pes_pd/assets/143084423/6eadd80b-2848-4d29-8c8e-8e9325ef7cf1)
  
  ![image](https://github.com/pavithra7369/pes_pd/assets/143084423/498e7194-861d-45e5-8e51-2ff7742aa211)

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
 4) STA (static timing analysis
      
+  Library characterization and modelling
  Libraries provide standardized building blocks that enhance design productivity and reusability, while characterization provides the essential data 
  needed to accurately model and simulate the behavior of these components, ensuring that the final design meets its performance, power, and reliability 
  goals.

+ Congestion aware placement using RePlAce
   > Placement is of 2 tyoes detailed placement and global placement
   ![WhatsApp Image 2023-09-16 at 14 46 07](https://github.com/pavithra7369/pes_pd/assets/143084423/6363b009-3d30-46a2-a3ad-e9214e838f55)
   ![WhatsApp Image 2023-09-16 at 14 47 23](https://github.com/pavithra7369/pes_pd/assets/143084423/bd7f5dea-5380-45df-9beb-91a6509d32fd)
   ![WhatsApp Image 2023-09-16 at 14 47 45](https://github.com/pavithra7369/pes_pd/assets/143084423/448568a8-e305-4d3f-87c3-1b14854c956f)
  > on further zooming
   ![WhatsApp Image 2023-09-16 at 14 48 21](https://github.com/pavithra7369/pes_pd/assets/143084423/6d8a15d9-ca9b-40c9-8fa4-82376c049d13)

 </details><details>
 <summary> Cell design and characterization flows
 </summary>

