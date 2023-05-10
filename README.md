# BGR
This github repository is for the design of a Band Gap Reference Circuit (BGR) using SCL_130nm technology PDK.
## Introduction to BGR
The Bandgap Reference (BGR) is a circuit which provides a stable voltage output which is independent of factors like temperature, supply voltage.

<img src="https://user-images.githubusercontent.com/110395336/221423844-50e88725-12dd-41e7-94af-d79a8e0f0e89.png" width="500" /> <img src="https://user-images.githubusercontent.com/110395336/221424115-dc0d6de7-a55b-470a-bcce-c31bdddd4d5a.png" width="500" />

## Features of Bandgap reference
1.  Temperature stability: A bandgap reference is designed to provide a stable reference voltage over a wide range of temperatures. This is achieved by using the temperature-dependent properties of semiconductors to cancel out the temperature dependence of other components in the circuit.
2. Low noise: A bandgap reference should have low noise to prevent any fluctuations in the reference voltage. This is achieved by using high-quality components and careful design.
3. High accuracy: A bandgap reference should have a high accuracy and precision to ensure that the reference voltage is consistent over time.
4. Low output impedance: A bandgap reference should have a low output impedance to provide a stable output voltage even when connected to a load.
5. Low power consumption: A bandgap reference should consume low power to minimize the impact on the overall system power consumption.
6. Wide input voltage range: A bandgap reference should be able to operate over a wide range of input voltages to ensure compatibility with different systems.
7. Small size: A bandgap reference should be compact in size to be easily integrated into different electronic systems.

## Why BGR and its Aplications
A bandgap reference is a circuit that uses the difference in voltage between the base-emitter junction and the semiconductor's bandgap voltage to produce a constant voltage reference that is unaffected by changes in temperature or power supply. Bandgap references are often used in the following contexts:
* Voltage regulation: Bandgap references are a reliable voltage reference that can be used in voltage regulators to control the output voltage of the regulator.
* Analog-to-digital converters (ADCs): ADCs require a stable and accurate voltage reference to convert analog signals into digital signals. Bandgap references can provide a stable reference voltage for ADCs.
* Voltage-controlled oscillators (VCOs): VCOs are used in frequency synthesizers, phase-locked loops, and other applications where a stable and accurate frequency is required. Bandgap references can be used to provide a stable reference voltage for the VCOs.
* Power management: Bandgap references can be used in power management applications such as battery chargers and power supplies to provide a stable voltage reference for controlling the output voltage.
* Temperature sensors: Bandgap references can be used in temperature sensors to provide a stable reference voltage that is independent of temperature variations.
* Digital-to-analog converters (DACs): DACs require a stable and accurate voltage reference to convert digital signals into analog signals. Bandgap references can provide a stable reference voltage for DACs.


# Table of contents
 - [1.BGR Introduction](#1-BGR-Introduction)
     - [1.1 BGR Principle](#11-BGR-Principle)
 - [2.CTAT voltage generation](#2-CTAT-voltage-generation)
     - [2.1 CTAT Voltage generation circuit](#21-CTAT-Voltage-generation-circuit)
     - [2.2 Voltage variation w.r.to temperature](#22-Voltage-variation-w.r.to-temperature)
     - [2.3 Slope of the Curve](#23-Slope-of-the-Curve)
     - [2.4 Function of CTAT using basic current mirror circuit](#24-CFunction-of-CTAT-using-basic-current-mirror-circuit)
     - [2.5 Voltages at specified nodes](#25-Voltages-at-specified-nodes)
 - [3.PTAT voltage generation](#3-PTAT-voltage-generation)
    - [3.1 PTAT using V2 and VD dependent](#31-PTAT-using-V2-and-VD-dependent)
    - [3.2 Realising PTAT voltage variation circuit independently](#32-Realisin-PTA-voltage-variation-circuit-independently)
    - [3.3 PTAT Voltage variation w.r.to temperature](#33-PTAT-Voltage-variation-w.r.to-temperature)
 - [4.Self-Biased Current Mirror Circuit](#4-Self-Biased-Current-Mirror-Circuit)
 - [5.Reference Branch Circuit](#5-Reference-Branch-Circuit)
    - [5.1 Synthesis](#51-Synthesis)
    - [5.2 Synthesizer](#52-Synthesizer)
 - [6.Start-up circuit](#6-Start-up-circuit)
 - [7.Complete BGR Circuit](#7-Complete-BGR-Circuit)
 - [8.SImulation and Waveforms](#8-SImulation-and-Waveforms)
    - [8.1 Voltage(Vref) variation wrt temperature(using Basic current mirror circuit)](#81-Voltage(Vref)-variation-wrt-temperature(using-Basic-current-mirror-circuit))
    - [8.2 Temperature coefficient(using Basic current mirror circuit)](#82-Temperature-coefficient(using-Basic-current-mirror-circuit))
    - [8.3 Voltage coefficient(using Basic current mirror circuit)](#83-Voltage-coefficient(using-Basic-current-mirror-circuit))
    - [8.4 BGR Circuit with Enable and Cascode current mirror circuit](#84-BGR-Circuit-with-Enable-and-Cascode-curren-mirror-circuit)
    - [8.5 Current and voltages are  identical for Cascode current mirror circuit](#85-GCurrent-and-voltages-are-identical-for-Cascode-current-mirror-circuit)
    - [8.6 PTAT and CTAT Cancelling each other to give constant Voltage (Vref) using Cascode current mirror circuit.](#86-PTAT-and-CTAT-Cancelling-each-other-to-give-constant-Voltage-(Vref)-using-Cascode-current-mirror-circuit.)
    - [8.7 Voltage variation w.r.to temperature(Cascode)](#87-Voltage-variation-w.r.to-temperature(Cascode))
    - [8.8 Voltage variation w.r.to VDD(Cascode)](#88-Voltage-variation-w.r.to-VDD(Cascode))
    - [8.9 Temperature coefficient](#89-Temperature-coefficient)
    - [8.10 Voltage coefficient](#810-Voltage-coefficient)
    - [8.11 Start-Up time](#811-Start-Up-time)
    - [8.12 On-Off current wrt to Enable](#812-On-Off-current-wrt-to-Enable)
    - [8.11 Customizing the layout](#86-Customizing-the-layout)
    - [8.6 Customizing the layout](#86-Customizing-the-layout)
 - [9. Specifications](#9-Specifications)
 - [10. Layout](#10-Layout)
 - [11. Future Work](#11-Future-Work)

## 1. BGR Introduction
### 1.1 BGR Principle
The operation principle of BGR circuits is to sum a voltage with negative temprature coefficient with another one exhibiting opposite temperature dependancies. Generally semiconductor diode behave as CTAT i.e. Complement to absolute temp. which means with increase in temp. the voltage across the diode will decrease. So we need to find a PTAT circuit which can cancel out the CTAT nature i.e. with rise in temp. the voltage across that device will increase and thus we can get a constant voltage reference with respect to temp.
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221425027-257a0ffa-1c41-4975-a2c7-e905343ebcfa.png'width="500"/>
</p>

## 2. CTAT voltage generation
CTAT (Temperature Coefficient of Absolute Temperature) voltage generation is a technique used to generate a voltage that is proportional to the absolute temperature. The CTAT voltage is typically generated using a diode or a transistor.One important consideration when designing a CTAT voltage generator is the temperature coefficient of the diode or transistor used in the circuit. The temperature coefficient determines how much the voltage across the diode or transistor will change with temperature, and it is important to choose a diode or transistor with a temperature coefficient that matches the desired temperature range of the circuit.

<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221425678-8f0c846d-2c72-4b12-b51f-71e797c786ab.png'width="500"/><img src='https://user-images.githubusercontent.com/110395336/221425684-a25ee02c-d1df-47ee-a4fb-e1fbde26f148.png'/>
</p>

### 2.1 CTAT Voltage generation circuit
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221425895-8a52140c-0c1b-4142-827d-f99a34d9e8e3.png'width="700"/>
</p>

### 2.2 Voltage variation w.r.to temperature
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221425933-889ffb88-7e2d-4f29-a114-ae48008fc37c.png'width="700"/>
</p>

### 2.3 Slope of the Curve
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221426000-c29f2b6e-1b29-4b4e-8679-a056988503b4.png'width="700"/>
</p>

### 2.4 Function of CTAT using basic current mirror circuit
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221426059-3c57fa78-8d2c-4413-9b49-60a10d99a178.png'width="700"/>
</p>

### 2.4 Voltages at specified nodes
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221426083-2df543b5-d632-4aae-a770-1bf3e2835907.png'width="700"/>
</p>

### 2.5 Current and voltages are almost identical(Basic current mirror circuit)
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221426110-3a598091-0e7d-4d67-93f3-e1dd2dcbc064.png'width="700"/>
</p>

## 3. PTAT voltage generation
PTAT stands for Proportional To Absolute Temperature and refers to a voltage or current that is proportional to the absolute temperature of a device or system. PTAT voltage generation is a technique used in electronic circuits to generate a voltage that is proportional to temperature. The PTAT voltage is typically generated by exploiting the temperature-dependent characteristics of a transistor or diode.In a typical PTAT voltage generator circuit, a transistor or diode is used as a temperature sensor. The voltage across the sensor is amplified and then fed into a voltage divider network to produce a voltage that is proportional to the absolute temperature of the sensor. 
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221426595-83907cd3-a7ee-4a9e-8c36-77d9ee9501d3.png'width="500"/><img src='https://user-images.githubusercontent.com/110395336/221426606-1eaa223d-204f-4edd-8fe9-984de0263e69.png'width="500"/>
</p>

### 3.1 PTAT using V2 and VD dependent
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221426704-fdde34d2-e2c1-4fec-9e9f-6d2c8a124664.png'width="700"/>
</p>

### 3.2 Realising PTAT voltage variation circuit independently
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221426729-1aa2810e-eecb-44bb-a861-7c11a7178ab5.png'width="700"/>
</p>

### 3.3 PTAT Voltage variation w.r.to temperature
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221426775-cd69abb1-7bd6-485d-9620-c6becdcef0ce.png'width="700"/>
</p>

## 4. Self-Biased Current Mirror Circuit
A self-biased current mirror circuit is a type of electronic circuit used in integrated circuits to replicate an input current to an output current with a high degree of accuracy. In a self-biased current mirror circuit, the bias current for the circuit is generated internally, eliminating the need for an external bias voltage.
The circuit typically consists of two transistors - a master transistor and a slave transistor - and a resistor. The master transistor is biased with a fixed current, and the slave transistor is biased using the voltage drop across the resistor. As a result, the output current of the slave transistor is a mirror image of the input current to the master transistor.
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221427036-e5da7603-c07f-495a-b973-80eac68f5bee.png'width="300"/>
</p>

## 5. Reference Branch Circuit
The reference circuit branch performs the addition of CTAT and PTAT volages and gives the final reference voltage. We are using a mirror transitor and a BJT as diode in the reference branch. By virtue of the mirror transistor in the reference branch the same amount of current flows through it as of the current mirror branches. Now from the PTAT circuit branch we are getting PTAT voltage and PTAT current. The same PTAT current is flowing in the reference branch. But the slope of PTAT voltage is much more smaller than that of slope of CTAT voltgae. In order to make increase the voltage slope we have to increase the resistance (current constant, so V increases with increase in R). Now across the high resistance we will get our constant reference voltage which is the result of CTAT Voltage + PTAT Voltage.

<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221430814-44df1bc2-a8d3-40b4-ad54-0e6a778b6a25.png'width="300"/>
</p>

### CTAT and PTAT cancelling each other to maintain voltage(Vref) unchanged with respect to temperature(Basic Current mirror)
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221427149-b16509cd-1d02-4ade-b01c-d139e60d9fd5.png'width="300"/><img src='https://user-images.githubusercontent.com/110395336/221427157-d59224b9-66b2-4b2e-bde5-02d31fff65a2.png'width="300"/>
</p>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/219571021-c8bedc9c-a021-4212-a437-855de57815ae.png'width="800"/>
</p>

## 6. Start-up circuit

The start-up circuit is required to move out the self biased current mirror from degenerative bias point (zero current). The start-up circuit forecefully flows a slow amount of current through the self-biased current mirror when the current is 0 in the current mirror branches, as the current mirror is self biased this small current creats a disturbance and the current mirror auto biased to the desired current value.
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221431106-61fb5c7b-562c-4f5b-b065-290e5567e2e9.png'width="300"/>
</p>


## 7. Complete BGR Circuit

Now by connecting all above components we can get the complete BGR circuit.
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/217156197-c168f612-c3e4-4d4c-894c-5d401b23509f.png'width="700"/>
</p>
<br>
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221431437-9cfd5f94-aba1-4f6c-a542-f5477a9b7b7b.png'/>
</p>
<br>

## 8. SImulation and Waveforms
### 8.1 Voltage(Vref) variation wrt temperature(using Basic current mirror circuit)
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/219571053-be63ac6e-b7fd-4c96-bd65-9b5802e4f8ad.png'width="1000"/>
</p>

### 8.2 Temperature coefficient(using Basic current mirror circuit)
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/219571528-bb2c1857-88e9-4552-b025-795827dd8ba5.png'width="1000"/>
</p>

### 8.3 Voltage coefficient(using Basic current mirror circuit)
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/219571578-ba3cd981-8cc0-448b-b79c-59b129a5a349.png'width="1000"/>
</p>

### 8.4 BGR Circuit with Enable and Cascode current mirror circuit
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221620887-2b403478-5c5f-40e4-9693-bb4f679265e1.png'width="1000"/>
</p>

### 8.5 Current and voltages are  identical for Cascode current mirror circuit
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221415823-c2544cf6-53e8-4f6c-bb6c-e5e10d517325.png'width="1000"/>
</p>

### 8.6 PTAT and CTAT Cancelling each other to give constant Voltage (Vref) using Cascode current mirror circuit.
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221415953-827bcf54-dcf1-47dd-94d4-d581bd71017e.png'width="1000"/>
</p>

### 8.7 Voltage variation w.r.to temperature(Cascode)
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221415997-87af5d3b-3013-4c41-b7b8-861a09544b18.png'width="1000"/>
</p>

### 8.8 Voltage variation w.r.to VDD(Cascode)
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221416036-c0f3f375-0a28-4979-8091-1b74fb5b164b.png'width="1000"/>
</p>

### 8.9 Temperature coefficient
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221416075-7db48ec5-e4ee-449b-9535-931a5822a222.png'width="1000"/>
</p>

### 8.10 Voltage coefficient
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221416213-a0fcf14e-2d5a-4db3-87b7-f1aaf8c81cc0.png'width="1000"/>
</p>

### 8.11 Start-Up time 
<br>
<p align='center'>
<![Screenshot from 2023-05-10 12-56-25](https://github.com/AjayKumar-Gandi/msvsdbgr/assets/110395336/a4a896e5-d251-4d6a-8686-34dbd3c1e25f)>
</p>

### 8.12 On-Off current wrt to Enable
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221416308-c5ce261c-ec1a-4eb4-a63f-c679bad99fd9.png'width="1000"/>
</p>

## 9. Specifications
<br>
<p align='center'>
<img src='https://user-images.githubusercontent.com/110395336/221602088-d03c24f2-9e39-4a41-9ee9-29e10348f791.png'width="900"/>
</p>

## 10. Layout
Band Gap Reference Circuit (BGR) Layout  

VLSI layout combines a huge number of circuits into a larger integrated circuit. There are 2 ways of doing a layout: manual and automated.  Manual layout usually enables the designer to pack his devices in a smaller area compared to the automated process but it is more tedious.  The automated process, on the other hand, is done using standard cells and usually takes more real estate space but it is much faster.  
The design rules which we will be using is the  SCL 130nm CMOS Rules. Design rules give guidelines for generating layouts. They dictate spacings between wells, sizes of contacts, minimum spacing beween a poly and a metal and many other similar rules. 

<p align='center'>
<img src= 'https://github.com/AjayKumar-Gandi/msvsdbgr/assets/110395336/08ec74f3-41e6-46e3-8f11-7fe6484b3d99.png' />  
</p>


<h4> Design of PNP (BJT) </h4>
<p align='center'>
<img src='https://github.com/AjayKumar-Gandi/msvsdbgr/assets/110395336/85f0dfac-7b9a-4e93-9dbc-fec5ad592ee7.png' />
</p>

<br>
<p align='center'>
<img src='https://github.com/AjayKumar-Gandi/msvsdbgr/assets/110395336/f8f5ef09-494e-4e05-a4f2-a4e7f68b8727.png' />
</p>
<br>
<h4> Transistor in Layout View </h4>
<p align='center'>
<img src='https://github.com/AjayKumar-Gandi/msvsdbgr/assets/110395336/1c1c5865-ce54-44ed-aea4-5882c1f19cba.png' />
</p>
<br>
<p align='center'>
<img src='https://github.com/AjayKumar-Gandi/msvsdbgr/assets/110395336/26c58b54-b423-42b1-960f-0e467bc09341.png' />
</p>
<br>
<br>
<p align='center'>
<img src='https://github.com/AjayKumar-Gandi/msvsdbgr/assets/110395336/3f44947d-daeb-499b-bc31-a8d498113444.png' />
</p>
<br>


## 11. Future Work

- Perform post layout simulation 

## Author

Gandi AjayKumar

## Acknowledgement

- Kunal Ghosh, Director, VSD Corp. Pvt. Ltd.
- Madhav Rao, Professor, IIIT-Bangalore.
- Nanditha Rao, Professor, IIIT-Bangalore.
- Semi-conductor Laboratory

## Contact Information

 - Gandi AjayKumar, M.Tech in VLSI Design, International Institute of Information Technology, Bangalore ajaykumar.gandi@iiitb.ac.in.
 - Kunal Ghosh, Director, VSD Corp. Pvt. Ltd. kunalghosh@gmail.com
 - Madhav Rao, Professor, IIIT-Bangalore. mr@iiitb.ac.in
 - Nanditha Rao, Professor, IIIT-Bangalore. nanditha.rao@iiitb.ac.in




