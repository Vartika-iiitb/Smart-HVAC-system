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
</details>

<details>
<summary>
References
</summary>
	
 * https://github.com/SakethGajawada/RISCV-GNU
 * https://github.com/kunalg123
 * https://www.vsdiat.com/
</details>



