# OPC Data Access & CentumVP DCS Introduction
This is a tutorial of showing how to collect data from OPC server which connected to DCS or PLC system. We also provide some reference tutorial to get your hands into Centum DCS. DCS is often seen in oil, gas, and chemica industries. 

# 1. Introduction <br />
Let's take a quick look of the system structure in the brochure from Yokogawa Centum VP. 
<p align="center">
<img src="/image/brochure_system_structure.JPG" height="90%" width="90%"> 
</p>  

We will only focus on HIS(Human Interface Station), ENG(Engineering Station) and FCS(Field Control Station) in this tutorial. The following system structure is what IT department would take care and within this tutorial's scope. You will notice OPC(Open Platform Communications) in the structure which play the role of exchanging data in the automation devices. This tutorial mainly focus on collecting data via OPC. 
<p align="center">
<img src="/image/IT_system_structure.jpg" height="60%" width="60%"> 
</p>  

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
            <td align="center">This is a small computer which can take analog or digital input/output and provide computing power. The communication between FCS and electronics or hardwares can be wired or wireless.</td>
        </tr>
        <tr>
            <td align="center">Human Interface Station (HIS)</td>
            <td align="center">DCS or remote office operators can monitor and take action on HIS to control DCS system. It includes some user interface, keyboard, and touchscreen.</td>
        </tr>
        <tr>
              <td align="center">Engineering Station (ENG)</td>
              <td align="center">Engineer, technichian, electrician, and developer can use this station to develop logic, program, user interface, chart, and so on. The program should be downloaded to FCS and HIS for functioning.</td>
        </tr>
        <tr>
              <td align="center">Open Platform Communications (OPC)</td>
              <td align="center">OPC can be devided into server and client. Server will be the one to communicate with FCS, while client will be the one to ask server to communicate and then receive data in FCS. Client can then process these data. There are multiple vendor (such as Kepware, OPC Expert, Advosol) who can provide OPC server software and corresponding library for development.</td>
        </tr>
    </tbody>
</table>
</p>

| Element      | Detail   
| -------------|------------- 
| Field Control Station </br>(FCS)           | This is a small computer which can take analog or digital input/output and provide computing power. </br>The communication between FCS and electronics or hardwares can be wired or wireless.  
| Human Interface Station </br>(HIS)         | DCS or remote office operators can monitor and take action on HIS to control DCS system. It includes </br>some user interface, keyboard, and touchscreen.
| Engineering Station </br>(ENG)             | Engineer, technichian, electrician, and developer can use this station to develop logic, program, user </br>interface, chart, and so on. The program should be downloaded to FCS and HIS for functioning.
| Open Platform Communications </br>(OPC)    | OPC can be devided into server and client. Server will be the one to communicate with FCS, while client </br>will be the one to ask server to communicate and then receive data in FCS. Client can then process these data. There are multiple vendor (such as Kepware, OPC Expert, Advosol) who can provide OPC server software and corresponding library for development.    

# 2. 
