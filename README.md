# Smart-HVAC-system

This Repository summarizes the progress made on RISC V based project.

<details>
  <summary>
    
  </summary>
  Heating, Ventilation, and Air Conditioning (HVAC) systems are essential in various applications where maintaining optimal indoor environmental conditions is crucial for comfort, health, or process requirements. HVAC systems are essential in various environments for several reasons:

**1. Comfort:**
Temperature Control: HVAC systems regulate indoor temperatures, ensuring occupants are comfortable regardless of external weather conditions.
Humidity Control: HVAC systems maintain optimal humidity levels, preventing discomfort caused by dry or excessively humid air.
**2. Health and Safety:**
Air Quality: HVAC systems filter and circulate air, removing pollutants, allergens, and contaminants. This is crucial for indoor air quality, especially in buildings with limited natural ventilation.
Disease Control: Proper ventilation and air exchange help reduce the spread of airborne diseases by diluting and exhausting contaminants.
**3. Energy Efficiency:**
Energy Conservation: HVAC systems are designed to be energy-efficient, reducing overall energy consumption in buildings.
Temperature Zoning: HVAC systems can be designed with zoning capabilities, allowing specific areas to be heated or cooled as needed, conserving energy in unoccupied spaces.
**4. Preservation:**
Preservation of Goods: In industries, HVAC systems help maintain stable temperature and humidity levels, preserving products, raw materials, and equipment.
Preservation of Artifacts: HVAC systems are critical in museums and archives to preserve artifacts, paintings, and historical documents.
**5. Productivity and Performance:**
Employee Productivity: Comfortable and healthy indoor environments enhance productivity and reduce absenteeism among employees.
Equipment Performance: HVAC systems prevent overheating of machinery and electronic equipment, ensuring they operate efficiently and have a longer lifespan.
**6. Control of Environmental Factors:**
Odor Control: HVAC systems can include filters and ventilation systems to control and eliminate odors, crucial in environments like restaurants and industrial facilities.
Smoke and Fume Extraction: HVAC systems in commercial kitchens and factories remove smoke, fumes, and airborne particles, providing a safer working environment.
**7. Compliance and Regulations:**
Regulatory Compliance: Many building codes and regulations require proper ventilation and HVAC systems to ensure the health and safety of occupants.
Occupancy Permits: Buildings need to meet HVAC requirements to obtain occupancy permits, ensuring the space is fit for human habitation or commercial use.
**8. Specialized Needs:**
Server Rooms: HVAC systems maintain optimal temperatures in server rooms, preventing overheating and ensuring continuous operation of computer systems.
Clean Rooms: Industries such as pharmaceuticals, electronics, and aerospace rely on HVAC systems to maintain sterile and controlled environments.
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
  
  output.o:     file format elf32-littleriscv


Disassembly of section .text:

00010074 <main>:
   10074:	fe010113          	add	sp,sp,-32
   10078:	00812e23          	sw	s0,28(sp)
   1007c:	02010413          	add	s0,sp,32
   10080:	001f7793          	and	a5,t5,1
   10084:	fef42423          	sw	a5,-24(s0)
   10088:	fe842703          	lw	a4,-24(s0)
   1008c:	00100793          	li	a5,1
   10090:	04f71863          	bne	a4,a5,100e0 <main+0x6c>
   10094:	fec42783          	lw	a5,-20(s0)
   10098:	04079463          	bnez	a5,100e0 <main+0x6c>
   1009c:	00200793          	li	a5,2
   100a0:	fef42223          	sw	a5,-28(s0)
   100a4:	ff900793          	li	a5,-7
   100a8:	fef42023          	sw	a5,-32(s0)
   100ac:	fe442783          	lw	a5,-28(s0)
   100b0:	fe042703          	lw	a4,-32(s0)
   100b4:	00ef7f33          	and	t5,t5,a4
   100b8:	00ff6f33          	or	t5,t5,a5
   100bc:	00100793          	li	a5,1
   100c0:	fef42623          	sw	a5,-20(s0)
   100c4:	ff700793          	li	a5,-9
   100c8:	fef42023          	sw	a5,-32(s0)
   100cc:	fec42783          	lw	a5,-20(s0)
   100d0:	fe042703          	lw	a4,-32(s0)
   100d4:	00ef7f33          	and	t5,t5,a4
   100d8:	00ff6f33          	or	t5,t5,a5
   100dc:	0540006f          	j	10130 <main+0xbc>
   100e0:	fe842783          	lw	a5,-24(s0)
   100e4:	f8079ee3          	bnez	a5,10080 <main+0xc>
   100e8:	fec42703          	lw	a4,-20(s0)
   100ec:	00100793          	li	a5,1
   100f0:	f8f718e3          	bne	a4,a5,10080 <main+0xc>
   100f4:	00100793          	li	a5,1
   100f8:	fef42223          	sw	a5,-28(s0)
   100fc:	ff900793          	li	a5,-7
   10100:	fef42023          	sw	a5,-32(s0)
   10104:	fe442783          	lw	a5,-28(s0)
   10108:	fe042703          	lw	a4,-32(s0)
   1010c:	00ef7f33          	and	t5,t5,a4
   10110:	00ff6f33          	or	t5,t5,a5
   10114:	fe042623          	sw	zero,-20(s0)
   10118:	ff700793          	li	a5,-9
   1011c:	fef42023          	sw	a5,-32(s0)
   10120:	fec42783          	lw	a5,-20(s0)
   10124:	fe042703          	lw	a4,-32(s0)
   10128:	00ef7f33          	and	t5,t5,a4
   1012c:	00ff6f33          	or	t5,t5,a5
   10130:	f51ff06f          	j	10080 <main+0xc>
   
   ```

![diff instru](https://github.com/Vartika-iiitb/Smart-HVAC-system/assets/140998716/afc21576-9b60-48fe-a481-0aa8330b8bdb)

Number of different instructions: 9
List of different instructions:
li
sw
or
bne
bnez
lw
and
j
add
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
* Divyam Satle, Colleague at IIITB
</details>

<details>
<summary>
References
</summary>
	
 * https://github.com/SakethGajawada/RISCV-GNU
 * https://github.com/kunalg123
 * https://www.vsdiat.com/
</details>



