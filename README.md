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
  int main() {
    int temp_sensor;      // bit 0
    int car_window_motor;//bit 1 & 2
    int AC;//bit 3
    int mask;

    

    while (1) {
        
	asm volatile(
	    	"andi %0, x30, 1\n\t"
	    	:"=r"(temp_sensor)
	    	:
	    	:
	    	);
        if (temp_sensor == 1 && AC==0) { // temperature more than threshold, Roll off the windows and turn on AC
            //motor=2 makes roll up windows
            car_window_motor=2;//10 is one direction
            
            mask = 0xFFFFFFF9;
            asm volatile(
	    "and x30, x30, %1\n\t"
	    "or x30, x30, %0\n\t"
	    :
	    :"r"(car_window_motor),"r"(mask)
	    :"x30"
	    );
            
            
            //on AC
            AC=1;
            mask = 0xFFFFFFF7;
            asm volatile(
	    "and x30, x30, %1\n\t"
	    "or x30, x30, %0\n\t"
	    :
	    :"r"(AC),"r"(mask)
	    :"x30"
	    );
        }
        else if (temp_sensor == 0 && AC==1)
        { // temperature less than threshold roll down windows & AC is off
           
              //motor=1 makes roll down windows
            car_window_motor=1;//01 is opposite direction
            
            mask = 0xFFFFFFF9;
            asm volatile(
	    "and x30, x30, %1\n\t"
	    "or x30, x30, %0\n\t"
	    :
	    :"r"(car_window_motor),"r"(mask)
	    :"x30"
	    );
            
            
            //on AC
            AC=0;
            mask = 0xFFFFFFF7;
            asm volatile(
	    "and x30, x30, %1\n\t"
	    "or x30, x30, %0\n\t"
	    :
	    :"r"(AC),"r"(mask)
	    :"x30"
	    );
        }

       
    }

    return 0;
}
```

</details>

<details>

  <summary>
    Code conversion to Assembly
  </summary>

```
  c.out:     file format elf32-littleriscv


Disassembly of section .text:

00010054 <main>:
   10054:	fe010113          	addi	sp,sp,-32
   10058:	00812e23          	sw	s0,28(sp)
   1005c:	02010413          	addi	s0,sp,32
   10060:	001f7793          	andi	a5,t5,1
   10064:	fef42423          	sw	a5,-24(s0)
   10068:	fe842703          	lw	a4,-24(s0)
   1006c:	00100793          	li	a5,1
   10070:	04f71863          	bne	a4,a5,100c0 <main+0x6c>
   10074:	fec42783          	lw	a5,-20(s0)
   10078:	04079463          	bnez	a5,100c0 <main+0x6c>
   1007c:	00200793          	li	a5,2
   10080:	fef42223          	sw	a5,-28(s0)
   10084:	ff900793          	li	a5,-7
   10088:	fef42023          	sw	a5,-32(s0)
   1008c:	fe442783          	lw	a5,-28(s0)
   10090:	fe042703          	lw	a4,-32(s0)
   10094:	00ef7f33          	and	t5,t5,a4
   10098:	00ff6f33          	or	t5,t5,a5
   1009c:	00100793          	li	a5,1
   100a0:	fef42623          	sw	a5,-20(s0)
   100a4:	ff700793          	li	a5,-9
   100a8:	fef42023          	sw	a5,-32(s0)
   100ac:	fec42783          	lw	a5,-20(s0)
   100b0:	fe042703          	lw	a4,-32(s0)
   100b4:	00ef7f33          	and	t5,t5,a4
   100b8:	00ff6f33          	or	t5,t5,a5
   100bc:	0540006f          	j	10110 <main+0xbc>
   100c0:	fe842783          	lw	a5,-24(s0)
   100c4:	f8079ee3          	bnez	a5,10060 <main+0xc>
   100c8:	fec42703          	lw	a4,-20(s0)
   100cc:	00100793          	li	a5,1
   100d0:	f8f718e3          	bne	a4,a5,10060 <main+0xc>
   100d4:	00100793          	li	a5,1
   100d8:	fef42223          	sw	a5,-28(s0)
   100dc:	ff900793          	li	a5,-7
   100e0:	fef42023          	sw	a5,-32(s0)
   100e4:	fe442783          	lw	a5,-28(s0)
   100e8:	fe042703          	lw	a4,-32(s0)
   100ec:	00ef7f33          	and	t5,t5,a4
   100f0:	00ff6f33          	or	t5,t5,a5
   100f4:	fe042623          	sw	zero,-20(s0)
   100f8:	ff700793          	li	a5,-9
   100fc:	fef42023          	sw	a5,-32(s0)
   10100:	fec42783          	lw	a5,-20(s0)
   10104:	fe042703          	lw	a4,-32(s0)
   10108:	00ef7f33          	and	t5,t5,a4
   1010c:	00ff6f33          	or	t5,t5,a5
   10110:	f51ff06f          	j	10060 <main+0xc>
   
   ```


```
Number of different instructions: 10
List of unique instructions:
j
or
addi
bne
lw
sw
and
andi
li
bnez


```
The compiled output of the C program has been shown below.

![tbovartika](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/792b1945-8da1-4bab-8915-3f00e0041b04)

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



