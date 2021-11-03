# OPC Data Access & CentumVP DCS Introduction
This is a tutorial of showing how to collect data from OPC server which connected to DCS or PLC system. We also provide some reference tutorial to get your hands into Centum DCS. DCS is often seen in energy, oil, gas, and chemica industries. The entire DCS solution can provide users to operate and monitor the manufacturing process in a remote office.

# 1. Introduction <br />
Let's take a quick look of the system structure in the brochure from Yokogawa Centum VP ([link](https://web-material3.yokogawa.com/BU33J01A10-01EN.pdf)) to know how does DCS embedded in a factory. 
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
            <td align="left">This is a small computer which can take analog or digital input/output and provide computing power. The communication between FCS and electronics, transmitter(flow, temperature, pressure), or hardwares (motor, actuator, pump) can be wired or wireless. Each input/output will be mapped to a memory address which is important when developing logic and program on ENG. FCS can be far away from the physical equipments and located in remote DCS office. </br> </br> In a large plant, you might have multiple FCS, each FCS will be assigned with an IP so that HIS, ENG, OPC can reach out to these FCS via network.</td>
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
1. Wire the electronics or hardwares to FCS.
2. Use ENG to develop programs and logic and download it to FCS so that FCS can process input and output signal.
3. Use ENG to develop user interface(UI) and download it to HIS operator station so that user can interact with DCS.
4. Use OPC server to collect FCS data on behalf of client program. The data can be stored in SQL database for later processing.

# 2. Human Interface Station (HIS)
Let's take a look at what a DCS operator would see on their screen on the HIS. You can see browser bar, graphics, some alarm, silo, piping, valve, pumps, and some values. The user interface program directly asks data from FCS or send command to the FCS therefore monitor and control the production process value. 
<p align="center">
<img src="/image/CENTUMVP-HMI.png" height="90%" width="90%"> 
</p>  
In a production line, there are large amount of devices and equipments. Usually, we will name each of them into some alphabet and numbers which can make the user interface more clean and neat and better management. 

