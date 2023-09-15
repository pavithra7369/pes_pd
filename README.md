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

# DAY 1

   </details>     
      <details> 
          <summary>   Inception of open-source EDA, OpenLANE and Sky130 PDK    </summary>

## How to talk to computers
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

> Die ->A die is a small block of semiconducting material on which a given functional circuit is fabricated.

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

## SoC design and OpenLANE
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
     
     
     


           
      

