# Smart-HVAC-system

This Repository summarizes the progress made on RISC V based project.

<details>
  <summary>
    Purpose
  </summary>
  Heating, Ventilation, and Air Conditioning (HVAC) systems are essential in various applications where maintaining optimal indoor environmental conditions is crucial for comfort, health, or process requirements. 

This Project focusses on smart HVAC systems that are installed in cars which will ensure a temperature and moisture control inside the vehicle using DHT11 temperature and moisture sensor. It is Effective as well as economically feasible. The output of the sensor will act as the input for RISC V which will be help us to roll off the window and Switch on the HVAC system even if the people are not around. These are useful when the car is either parked on outdoors during the day time or if there is humidity out. These are essential in various environments for several reasons:

**1. Comfort:**

Temperature Control: HVAC systems regulate indoor temperatures, ensuring occupants are comfortable regardless of external weather conditions.
Humidity Control: HVAC systems maintain optimal humidity levels, preventing discomfort caused by dry or excessively humid air.

**2. Health and Safety:**

Air Quality: HVAC systems filter and circulate air, removing pollutants, allergens, and contaminants. This is crucial for indoor air quality, especially in buildings with limited natural ventilation.
Disease Control: Proper ventilation and air exchange help reduce the spread of airborne diseases by diluting and exhausting contaminants.

**3. Energy Efficiency:**
Energy Conservation: HVAC systems are designed to be energy-efficient, reducing overall energy consumption in buildings.
Temperature Zoning: HVAC systems can be designed with zoning capabilities, allowing specific areas to be heated or cooled as needed, conserving energy in unoccupied spaces.

In summary, HVAC systems are essential for ensuring human comfort, health, safety, and the efficient operation of buildings and various industrial processes. They are designed to address a wide range of environmental and operational needs in diverse settings.
  
</details>

<details>
  <summary>
    MOTIVATION
  </summary>
  This Project focusses on creating a  smart HVAC system in cars. The development and integration of HVAC (Heating, Ventilation, and Air Conditioning) systems in cars are driven by several important factors, all aimed at enhancing the comfort, safety, and overall driving experience for passengers and drivers:

  **1. Passenger Comfort:**
  
 (a) Temperature Control: HVAC systems allow passengers to maintain a comfortable temperature inside the car, regardless of the weather conditions outside. This is especially important during extreme heat or cold.
(b) Humidity Control: Proper ventilation helps control humidity levels, preventing the feeling of stickiness and discomfort inside the vehicle.

**2. Driver Comfort and Safety:**

(a) Fog and Defrosting: HVAC systems are crucial for defrosting windows during cold weather. They also help prevent fogging, ensuring optimal visibility for the driver, which is essential for safe driving.
(b) Dehumidification: HVAC systems dehumidify the air, preventing the buildup of condensation inside the vehicle. This is especially important in preventing fogging on windows.
Occupant Focus: Comfortable passengers are less likely to distract the driver, contributing to overall road safety.

**3. Health and Well-being:**

Comfortable Journey: A comfortable temperature and clean air contribute to reduced stress during travel, enhancing the overall well-being of passengers.
Preventing Overheating: In hot weather, an efficient air conditioning system prevents passengers, especially children and the elderly, from overheating, which can be dangerous.

**4. Market Demand and Competitiveness:**

Consumer Expectations: Modern consumers expect a high level of comfort and convenience in their vehicles. HVAC systems have become a standard feature in most vehicles to meet these expectations.
Competitive Advantage: Car manufacturers compete based on the features and comfort they offer. A well-designed HVAC system adds value to the vehicle and can be a competitive advantage in the market.

**5. Vehicle Functionality:**

Demands of Modern Vehicles: Modern vehicles often come with electronic systems and gadgets that generate heat. Efficient HVAC systems help dissipate this heat, ensuring the proper functioning of these components.
Battery Cooling: In electric and hybrid vehicles, HVAC systems are used to cool batteries, ensuring they operate within the optimal temperature range.

**6. Regulatory Compliance:**

Emission Regulations: Regulations and standards often mandate the use of HVAC systems to control emissions and ensure efficient fuel consumption.
Safety Regulations: Proper defrosting and demisting are essential for compliance with safety regulations, ensuring visibility is not compromised.
In summary, the integration of HVAC systems in cars is driven by the need to provide comfort, safety, and well-being for passengers and drivers. Meeting consumer expectations, ensuring safety compliance, and staying competitive in the market are significant motivators for car manufacturers to invest in advanced and efficient HVAC technologies.

</details>
<details>
  <summary>
    BLOCK DIAGRAM
  </summary>
  
![WhatsApp Image 2023-10-10 at 19 18 57](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/5ea4909d-0650-4f72-9b06-aafc581a5e83)

</details>
<details>
  <summary>
    C code
  </summary>
	
  ```
#include<stdio.h>
int main()  {

    int sensor_status;
    int Temp_sensor;
    int masking;
    int i;
    int sensor_state;
    int HVAC0,HVAC1;
    
    
        //for (int j=0; j<15;j++) {
        while(1){
        
	
       //if(j<10)
			sensor_status=1;
	//else
			sensor_status=0;
			

		asm volatile(
		"or x30, x30, %1\n\t"
		"andi %0, x30, 0x01\n\t"
		: "=r" (sensor_state)
		: "r" (sensor_status)
		: "x30"
		);
	

 
       
        if (sensor_status == 0) {
            // If temp. is below threshold value keep AC off and windows closed
            masking=0xFFFFFFFD;
            //printf("AC off and windows closed \n");
          Temp_sensor = 0; 
       
            asm volatile(
            "and x30,x30, %0\n\t"     // Load immediate 1 into x30
            "ori x30, x30,2"                 // output at 2nd bit , switches off the HVAC unit
            :
            :"r"(masking)
            :"x30"
            );
            asm volatile(
	    	"addi %0, x30, 0\n\t"
	    	:"=r"(HVAC0)
	    	:
	    	:"x30"
	    	);
    	//printf("HVAC0 = %d\n",HVAC0);
            
      
        } 
        else {
            // If Temparture is above threshold value, turn on AC and rolloff windows for a while.
            masking=0xFFFFFFFD;
             Temp_sensor = 1; 
           //printf("AC turned on and windows opened for a while \n ");
            asm volatile( 
            "and x30,x30, %0\n\t"     // Load immediate 1 into x30
            "ori x30, x30,0"            //// output at 2nd bit , switches on the HVAC unit
            :
            :"r"(masking)
            :"x30"
        );
        asm volatile(
	    	"addi %0, x30, 0\n\t"
	    	:"=r"(HVAC1)
	    	:
	    	:"x30"
	    	);
	//printf("HVAC1 = %d\n",HVAC1);
        }

 //printf("Temp_sensor=%d \n", Temp_sensor);   

}

return 0;
}

```

</details>

<details>

  <summary>
    Code conversion to Assembly
  </summary>

```vartika:     file format elf32-littleriscv


Disassembly of section .text:

00010054 <main>:
   10054:	fd010113          	addi	sp,sp,-48
   10058:	02812623          	sw	s0,44(sp)
   1005c:	03010413          	addi	s0,sp,48
   10060:	00100793          	li	a5,1
   10064:	fef42623          	sw	a5,-20(s0)
   10068:	fe042623          	sw	zero,-20(s0)
   1006c:	fec42783          	lw	a5,-20(s0)
   10070:	00ff6f33          	or	t5,t5,a5
   10074:	001f7793          	andi	a5,t5,1
   10078:	fef42423          	sw	a5,-24(s0)
   1007c:	fec42783          	lw	a5,-20(s0)
   10080:	02079463          	bnez	a5,100a8 <main+0x54>
   10084:	ffd00793          	li	a5,-3
   10088:	fef42223          	sw	a5,-28(s0)
   1008c:	fe042023          	sw	zero,-32(s0)
   10090:	fe442783          	lw	a5,-28(s0)
   10094:	00ff7f33          	and	t5,t5,a5
   10098:	002f6f13          	ori	t5,t5,2
   1009c:	000f0793          	mv	a5,t5
   100a0:	fcf42e23          	sw	a5,-36(s0)
   100a4:	fbdff06f          	j	10060 <main+0xc>
   100a8:	ffd00793          	li	a5,-3
   100ac:	fef42223          	sw	a5,-28(s0)
   100b0:	00100793          	li	a5,1
   100b4:	fef42023          	sw	a5,-32(s0)
   100b8:	fe442783          	lw	a5,-28(s0)
   100bc:	00ff7f33          	and	t5,t5,a5
   100c0:	000f6f13          	ori	t5,t5,0
   100c4:	000f0793          	mv	a5,t5
   100c8:	fcf42c23          	sw	a5,-40(s0)
   100cc:	f95ff06f          	j	10060 <main+0xc>
   
   ```


```
Number of different instructions: 11
List of unique instructions:
and
mv
andi
or
lw
bnez
li
addi
j
ori
sw
```


The compiled output of the C program has been shown below.

![tbovartika](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/792b1945-8da1-4bab-8915-3f00e0041b04)

</details>

<details>
	<summary>
		Spike Simulation
	</summary>
	The modified C code for Spike Simulation is shown below.

 ```
#include<stdio.h>
int main()  {

    int sensor_status;
    int Temp_sensor;
    int masking;
    int i;
    int sensor_state;
    int HVAC0,HVAC1;
    
    
        for (int j=0; j<15;j++) {
        //while(1){
        
	
       if(j<10)
			sensor_status=1;
	else
			sensor_status=0;
			

		asm volatile(
		"or x30, x30, %1\n\t"
		"andi %0, x30, 0x01\n\t"
		: "=r" (sensor_state)
		: "r" (sensor_status)
		: "x30"
		);
	

 
       
        if (sensor_status == 0) {
            // If temp. is below threshold value keep AC off and windows closed
            masking=0xFFFFFFFD;
            printf("AC off and windows closed \n");
          Temp_sensor = 0; 
       
            asm volatile(
            "and x30,x30, %0\n\t"     // Load immediate 1 into x30
            "ori x30, x30,2"                 // output at 2nd bit , keeps buzzer in off
            :
            :"r"(masking)
            :"x30"
            );
            asm volatile(
	    	"addi %0, x30, 0\n\t"
	    	:"=r"(HVAC0)
	    	:
	    	:"x30"
	    	);
    	printf("HVAC0 = %d\n",HVAC0);
            
      
        } 
        else {
            // If Temparture is above threshold value, turn on AC and rolloff windows for a while.
            masking=0xFFFFFFFD;
             Temp_sensor = 1; 
           printf("AC turned on and windows opened for a while \n ");
            asm volatile( 
            "and x30,x30, %0\n\t"     // Load immediate 1 into x30
            "ori x30, x30,0"            //// output at 2nd bit , switches on the HVAC unit
            :
            :"r"(masking)
            :"x30"
        );
        asm volatile(
	    	"addi %0, x30, 0\n\t"
	    	:"=r"(HVAC1)
	    	:
	    	:"x30"
	    	);
	printf("HVAC1 = %d\n",HVAC1);
        }

 printf("Temp_sensor=%d \n", Temp_sensor);   

}

return 0;
}
```

When the temperature is above the threshold value, Then the Temp_sensor = 1, which inturns switches the AC and rolls off the windows for a while, contrary to that when temperature is below threshold value AC is off and windows are closed, the output of which is replicated in the spike results.
	
![Screenshot from 2023-10-25 17-12-34](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/997e8f6a-0958-419c-b313-e307f441cb2f)

![Screenshot from 2023-10-25 17-12-38](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/55e4e447-d0bf-4d29-89bb-104d6a70ec17)

to perform the spike simulations following commands were used

```
riscv64-unknown-elf-gcc -march=rv64i -mabi=lp64 -ffreestanding -o out HAVC_Ccode.c
spike pk out
```
</details>
<details>
	<summary>
		Functional Simulation
	</summary>
In this step we are performing functional simulation in order to check the working of our verilog code and the processor code. the functionality of different input conditions have been tested and verified with the desired output conditions.

The fig shown below clearly depicts the change in output with the change in input. In this input value = 0 shows the temperature is below the threshold value whereas the input value = 1, shows the the temperature is above the threshold value.


![io_imagegtkwave](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/88fee338-8b9f-45af-a96f-cf924ab0760f)

Toggling in the input depicts in the output as well.
The figure shown below depicts the clk, top_gpio_pins, output_gpio_pins, ID_instruction_type.
![Screenshot from 2023-10-28 17-15-14](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/1fa5dbe1-d975-4f2b-970e-e72a5953775d)

</details>

<details>
	<summary>
		Gate Level Simulation
	</summary>
In GAte Level Synthesis process, Initially we commented out the sram modules and removed the data and memory instructions because sky130_sram_1kbyte_1rw1r_32x512_8 is the standard cell that we used. After making the following changes in the processor code, we used the following command to convert the RTL code to netlist using Yosys. For that we first need to invoke yosys and then write the following commands.
	
```
$ yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80_512.lib 
read_verilog processor1.v 
synth -top wrapper
dfflibmap -liberty sky130_fd_sc_hd__tt_025C_1v80_512.lib 
abc -liberty sky130_fd_sc_hd__tt_025C_1v80_512.lib
write_verilog synth_test_processor.v

```
Here as we can depict from the screenshot given below there are two sky130_sram_2kbyte_1rwlr_32*512_8_data and the other one as sky130_sram_2kbyte_1rwlr_32*512_8_inst. 


 ![Screenshot from 2023-11-01 16-16-16](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/18e1c17c-fd56-4fb1-bd1e-2bb382a8e5f9)

To validate the UART functionality we need to make the writing_inst_done = 0 in the processor file. By doing this we are excluding the UART module from simulation.

For GLS, we need to make writing_inst_done = 1 in the processor file. we mapped the memory element of our design to sky130_sram_1kbyte_1rw1r_32x512_8.v file nad then using the commands shown below we can generate the netlist.

```
$ yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80_512.lib 
read_verilog processor2.v 
synth -top wrapper
dfflibmap -liberty sky130_fd_sc_hd__tt_025C_1v80_512.lib 
abc -liberty sky130_fd_sc_hd__tt_025C_1v80_512.lib
write_verilog synth_test_processor.v

```
FOr GLS,
To verify the functionality of the GLS using the iverilog command which includes sram modules and related sky130 primitives.
The Commands are shown below:

```
iverilog -o test testbench.v synth_test.v sky130_sram_1kbyte_1rw1r_32x256_8.v sky130_fd_sc_hd.v primitives.v
./test
gtkwave waveform.vcd &
```

![Screenshot from 2023-11-01 15-59-53](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/1fcc0b41-f62f-4cad-920f-df48c0447474)

To show the netlist following commands are being used.
```
show wrapper
```

Below image shows the netlist generated with highlighted wrapper module.

![Screenshot from 2023-11-01 16-03-58](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/e18f15cf-47a2-4309-a711-c0b825383e50)

 
</details>

<details>
	<summary>
		PHYSICAL DESIGN
	</summary>
	
**OpenLane**

OpenLane is an open-source, automated RTL-to-GDSII (Register Transfer Level to Graphic Data System II) flow for digital integrated circuit (IC) design. OpenLane aims to provide a complete open-source digital ASIC (Application-Specific Integrated Circuit) implementation flow, covering various stages of the chip design process, including synthesis, place and route, and final layout generation. The project is designed to be customizable and configurable to accommodate different design requirements.

**Preparing the Design**
Preparing the design and including the lef files: The commands to prepare the design and overwite in a existing run folder the reports and results along with the command to include the lef files is given below:

```
make mount
%./flow.tcl -interactive
% package require openlane 0.9
% prep -design project 

```

![Screenshot from 2023-11-16 22-20-49](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/dbb4ebe8-c975-4944-a54b-8231854bb45f)


 ## Synthesis
 
 To synthesize the code run the following command:
 
 ```
run_synthesis
```

![Screenshot from 2023-11-16 22-31-37](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/c9a0b2f5-dcbb-465e-b63e-01e3ede19f53)

**Statistics after Synthesis**

![Screenshot from 2023-11-16 23-34-22](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/e43dafd7-3234-4fcd-90be-ce4ced8835fb)


## Floorplan

In the context of ASIC (Application-Specific Integrated Circuit) design, a floorplan is a crucial step in the physical design process. It involves planning the arrangement and organization of various functional blocks and components on the silicon die. The floorplan plays a significant role in determining the overall performance, power consumption, and ease of manufacturing of the integrated circuit.


Following command helps to run floorplan:

```
run_floorplan
```

![Screenshot from 2023-11-16 22-48-22](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/be859868-609c-4c90-9628-10d4d91203cf)

Post the floorplan run, a .def file will have been created within the results/floorplan directory. We may review floorplan files by checking the floorplan.tcl.
To view the floorplan: Magic is invoked after moving to the results/floorplan directory,then use the floowing command:

```
magic -T /home/vartika/.volare/volare/sky130/versions/1341f54f5ce0c4955326297f235e4ace1eb6d419/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read wrapper.def &
```

![Screenshot from 2023-11-17 01-18-37](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/553b1dae-c88b-4137-b9f8-6126301ac39d)

## Die area (post floor plan)

![Screenshot from 2023-11-17 00-38-23](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/b2d7c80c-36cd-41e1-9a0c-e5e36040d3af)

## Core area (post floor plan)

![Screenshot from 2023-11-17 00-38-29](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/2b4ee13d-312d-4cb9-9994-de1e513ada14)



## Placement

```
run_placement
```

![Screenshot from 2023-11-16 23-04-06](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/e4607bec-b49d-4c3c-a111-c686dfd194f2)

Post placement: the design can be viewed on magic within results/placement directory. Run the follwing command in that directory:

```
magic -T /home/vartika/.volare/volare/sky130/versions/1341f54f5ce0c4955326297f235e4ace1eb6d419/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read wrapper.def &
```

![Screenshot from 2023-11-16 23-56-00](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/ec3fed00-9c04-4a87-aac7-e0139a77a7c8)

Post placement: the design can be viewed on magic within results/placement directory. Run the follwing command in that directory:

```
magic -T /home/vartika/.volare/volare/sky130/versions/1341f54f5ce0c4955326297f235e4ace1eb6d419/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read wrapper.def &
```
## CTS

Clock Tree Synthesis (CTS) is a crucial step in the physical design flow of an ASIC (Application-Specific Integrated Circuit). It involves the generation of a well-optimized and balanced clock distribution network across the chip to ensure precise and synchronous clocking of various components. The purpose of building a clock tree is enable the clock input to reach every element and to ensure a zero clock skew. H-tree is a common methodology followed in CTS.



```
run_cts
```

![Screenshot from 2023-11-16 23-14-44](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/a8fc8131-2322-4f2b-bdd5-f52953bb6c71)

Below is the power report attached 
![Screenshot from 2023-11-17 01-27-13](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/80be78d2-b718-4f2e-a83a-94a911bbd0aa)

The timing report of circuit is 

![Screenshot from 2023-11-17 01-28-02](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/2abb6f15-114b-4143-b2f6-8f92ad7896cc)

## Routing

Routing in VLSI refers to the process of establishing physical connections between various components and modules on an integrated circuit (IC) chip. These connections are necessary to enable the flow of signals between different functional blocks, such as logic gates, memory cells, and input/output pads. Routing is a critical step in the physical design of an IC and plays a significant role in determining the overall performance, power consumption, and manufacturability of the chip.

```
run_routing
```


![Screenshot from 2023-11-17 00-32-18](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/572f39bc-1c97-4dda-b4f5-392d72192dfe)

**In Routing Stage**

Layout in magic tool post routing: the design can be viewed on magic within results/routing directory. Run the follwing command in that directory:

```
magic -T /home/vartika/.volare/volare/sky130/versions/1341f54f5ce0c4955326297f235e4ace1eb6d419/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read wrapper.def &
```
**Layout after Routing**

![Screenshot from 2023-11-17 18-20-32](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/9d57e253-423c-48c6-846b-41405f840d28)

**Area of Design**

![Screenshot from 2023-11-17 18-24-43 (1)](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/93a5d22b-c33f-486e-9d71-d5a1f83576bf)


**DRC**

![Screenshot from 2023-11-17 19-28-32](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/9ee731f0-5aa3-468c-b520-2052a779bb31)

</details>
<details>
  <summary>
    FUTURE SCOPE
  </summary>
  
 * To develop a user-friendly interface accessible via a mobile app, web dashboard or both.
 * It include features like real time temperature monitoring, Scheduling and remote control.
 * Implement Machine Learning algorithms
 
</details>

<details>
<summary>
Acknowldgement
</summary>

* I would sincerely like to thank Mr. Kunal Ghosh, Co founder of VLSI System Design Corp. Pvt. Ltd. for his consistent support and guidance throughout this task.
* Bhargav, Colleague at IIITB
* Mayank Kabra (Founder, Chipcron Pvt. Ltd.)
</details>

<details>
<summary>
References
</summary>
	
 * https://github.com/SakethGajawada/RISCV-GNU
 * https://github.com/kunalg123
 * https://www.vsdiat.com/
</details>



