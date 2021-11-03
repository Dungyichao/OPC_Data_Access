# OPC Data Access & Centum VP DCS Introduction
This is a tutorial of showing how to collect data from OPC server which connected to Distributed Control System(DCS) or PLC system. We also provide some reference tutorial to get your hands into Centum DCS. DCS is often seen in energy, oil, gas, and chemica industries. The entire DCS solution can provide users to operate and monitor the manufacturing process in a remote office.

# 1. Introduction <br />
Let's take a quick look of the system structure in the brochure from Yokogawa Centum VP ([link](https://web-material3.yokogawa.com/BU33J01A10-01EN.pdf),[PDF](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/CentumVP_Brochure_BU33J01A10-01EN.pdf)) to know how does DCS embedded in a factory. Yokogawa is an industry well known DCS solution pioneer originated in Japan since 1915. Other well known DCS solution provider such as Siemens, Honeywell, ABB, Rockwell, GE, Mitsubishi.
<p align="center">
<img src="/image/brochure_system_structure.JPG" height="90%" width="90%"> 
</p>  

We will only focus on HIS(Human Interface Station), ENG(Engineering Station) and FCS(Field Control Station) in this tutorial. The following system structure is what IT department would take care and within this tutorial's scope. You will notice OPC(Open Platform Communications) in the structure which play the role of exchanging data in the automation devices. This tutorial mainly focus on collecting data via OPC. 
<p align="center">
<img src="/image/IT_system_structure.jpg" height="60%" width="60%"> 
</p>  

## 1.1 Elements in DCS Solution
What is the role and relationship between each elements? Let's take a look at following table.

<p align="center">
<table>
    <thead>
        <tr>
            <th align="center">Element</th>
            <th align="center">Detail</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td align="center">Field Control Station (FCS)</td>
            <td align="left">This is a small computer which can take analog or digital input/output and provide computing power (like our PC take keyboard, mouse as input and output to monitor). The communication between FCS and electronics, transmitter(flow, temperature, pressure), or hardwares (motor, actuator, pump) can be wired or wireless. Each input/output will be mapped to a memory address which is important when developing logic and program on ENG. FCS can be far away from the physical equipments and located in remote DCS office. </br> </br> In a large plant, you might have multiple FCS, each FCS will be assigned with an IP so that HIS, ENG, OPC can reach out to these FCS via network.</td>
        </tr>
        <tr>
            <td align="center">Human Interface Station (HIS)</td>
            <td align="left">DCS or remote office operators can monitor and take action on HIS to control DCS system. It includes some user interface, keyboard, and touchscreen.</td>
        </tr>
        <tr>
              <td align="center">Engineering Station (ENG)</td>
              <td align="left">Engineer, technichian, electrician, and developer can use this station to develop logic, program, user interface, chart, and so on. The program should be downloaded to FCS and HIS for functioning.</td>
        </tr>
        <tr>
              <td align="center">Open Platform Communications (OPC)</td>
              <td align="left">OPC can be devided into server and client. Server will be the one to communicate with FCS, while client will be the one to ask server to communicate and then receive data in FCS. Client can then process these data. There are multiple vendor (such as Kepware, OPC Expert, Advosol) who can provide OPC server software and corresponding library for development. </br> </br>These vendors should provide industry standard, so the program you developed should be able to run on different OPC server with a little twist. For example, the program I developed on Kepserver (Kepware) to collect data can also run on Yokogawa's EXAOPC server when appyling vendor's library. You can use VB or .NET to develop client program.</td>
        </tr>
    </tbody>
</table>
</p>

## 1.2 Build a DCS Solution
The main process to build a DCS solution is as following:
1. Wire the electronics or hardwares from the field or production line into FCS.
2. Use ENG to develop programs and logic and download it to FCS so that FCS can process input and output signal.
3. Use ENG to develop user interface(UI) and download it to HIS operator station so that user can interact with DCS.
4. Use OPC server to collect FCS data on behalf of client program. The data can be stored in SQL database for later processing.

# 2. Human Interface Station (HIS)
Let's take a look at what a DCS operator would see on their screen on the HIS. You can see browser bar, graphics, some alarm, silo, piping, valve, pumps, and some values. The graph abstract the production process and equipments. The user interface program directly asks data from FCS or send command to the FCS therefore monitor and control the production process value. 

<p align="center">
<img src="/image/CENTUMVP-HMI.png" height="90%" width="90%"> 
</p>  

## 2.1 Equipment Naming Practice
In a production line, there are large amount of controller, devices and equipments. Usually, we will name each of them into tag name(some alphabet and numbers) which can make the user interface more clean and neat and better management. The nameing convention can be seen in the following ([Reference](https://www.controleng.com/articles/plc-tag-and-address-naming-conventions/), [PDF](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/Control%20Engineering%20_%20PLC%20tag%20and%20address%20naming%20conventions.pdf))

<p align="center">
<table>
    <thead>
        <tr>
            <th align="center">Devices</th>
            <th align="center">Alphabetic Abbreviation</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td align="center">Flow Transmitter</td>
            <td align="center">FT</td>
        </tr>
        <tr>
            <td align="center">Valve</td>
            <td align="center">HV, FV</td>
        </tr>
        <tr>
            <td align="center">Limit Switch</td>
            <td align="center">LSL (Low), LSH (High)</td>
        </tr>
        <tr>
            <td align="center">Loop Control</td>
            <td align="center">FIC, PIC</td>
        </tr>
        <tr>
            <td align="center">Pushbutton/Switch</td>
            <td align="center">HS, HIS</td>
        </tr>
        <tr>
            <td align="center">Photoeye, Proximity Switch</td>
            <td align="center">ZS</td>
        </tr>
        <tr>
            <td align="center">Motor Starter</td>
            <td align="center">M</td>
        </tr>
        <tr>
            <td align="center">Pressure Transmitter</td>
            <td align="center">PT, PIT</td>
        </tr>
        <tr>
            <td align="center">Temperature Indicating Control</td>
            <td align="center">TIC</td>
        </tr>
    </tbody>
</table>
</p>

For example, we have 25 Temperature Indicater on the production line, we therefore name the first one as TIC001, and the second one as TIC002, .... and all the way to TIC025.

## 2.2 Interprete Values on the Screen
Let's first see a simple example. We have an equipment, car(tag name: CAR001). We want to have the car to run at 50 mph (<b>set point value, SV</b>) no matter on what kind of surrounding.  Car is stop at the begining without break. By pushing the gas pedal 50%, the real time car speed (<b>process variable, PV</b>) will gradually and quickly start from 0 MPH to 50 MPH, then we can mantain the car speed 50 mph on a flat road with gas pedal 30%. However, when the car encounter a hill, the car will become slower (PV will go down probably to 35 mph) if the gas pedal still at 30%. In order for the car to reach the set point value(SV, 50 mph) during the uphill, the loop control will take in charge and press more on the gas pedal (probably 60%) so that we can mantain the car speed to 50 mph.

<p align="center">
<img src="/image/interprete_value_car.jpg" height="70%" width="70%"> 
</p>  

Likewise, on the HIS user interface, you can see tag name represents the equipment. PV indicating the current process variable of the equipment, and the SV indicating the set value from the user. For the car example, the whole tag name for the process variable will become CAR001.PV and the set value will be CAR001.SV. You might also see MV which represent maneuver value. You can think of MV as the effort for FCS to make PV close to SV, for example 30% or 50% on the gas pedal.

