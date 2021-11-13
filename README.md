# Centum VP DCS Introduction & OPC Data Access 
This is a tutorial of showing how to collect data from OPC server which connected to Distributed Control System(DCS) or PLC system. We also provide some reference tutorial to get your hands into Centum DCS. DCS solution is often seen in energy, oil, gas, and chemica industries. The entire DCS solution can provide users to operate and monitor the manufacturing process in a remote office.

# Table of Contents
1. [Introduction](https://github.com/Dungyichao/OPC_Data_Access#1-introduction-)
	* 1.1 [Elements in DCS Solution](https://github.com/Dungyichao/OPC_Data_Access#11-elements-in-dcs-solution)
	* 1.2 [Build a DCS Solution](https://github.com/Dungyichao/OPC_Data_Access#12-build-a-dcs-solution)
2. [What You See - Human Interface Station (HIS)](https://github.com/Dungyichao/OPC_Data_Access#2-what-you-see---human-interface-station-his)
	* 2.1 [Equipment Naming Practice](https://github.com/Dungyichao/OPC_Data_Access#21-equipment-naming-practice)
	* 2.2 [Interprete Values on the Screen](https://github.com/Dungyichao/OPC_Data_Access#22-interprete-values-on-the-screen)
		* 2.2.1 [PV, SV, MV](https://github.com/Dungyichao/OPC_Data_Access#221-pv-sv-mv)
		* 2.2.2 [AUT, MAN, CAS](https://github.com/Dungyichao/OPC_Data_Access#222-aut-man-cas)
3. [Behind the Scenes - Engineering Station (ENG)](https://github.com/Dungyichao/OPC_Data_Access/blob/main/README.md#3-behind-the-scenes---engineering-station-eng)
	* 3.1 [Basic User Interface Introduction (video)](https://github.com/Dungyichao/OPC_Data_Access#31-basic-user-interface-introduction-video)
	* 3.2 [Design and Hardware Relationship](https://github.com/Dungyichao/OPC_Data_Access/blob/main/README.md#32-design-and-hardware-relationship)
	* 3.3 [Project System View](https://github.com/Dungyichao/OPC_Data_Access/blob/main/README.md#33-project-system-view)
4. [How We Collect - OPC Data Access](https://github.com/Dungyichao/OPC_Data_Access#4-how-we-collect---opc-data-access)
	* 4.1 [Connect to OPC Steps](https://github.com/Dungyichao/OPC_Data_Access#41-connect-to-opc-steps)
	* 4.2 [Codes](https://github.com/Dungyichao/OPC_Data_Access#42-codes)
	* 4.3 [Make Console Application to Service](https://github.com/Dungyichao/OPC_Data_Access#43-make-console-application-to-service) (optional)
		* 4.3.1 [Convert the App](https://github.com/Dungyichao/OPC_Data_Access#431-convert-the-app)
		* 4.3.2 [Register and run the service](https://github.com/Dungyichao/OPC_Data_Access#432-register-and-run-the-service)	


# 1. Introduction <br />
Let's take a quick look of the system structure in the brochure from Yokogawa Centum VP ([link](https://web-material3.yokogawa.com/BU33J01A10-01EN.pdf),[PDF](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/CentumVP_Brochure_BU33J01A10-01EN.pdf)) to know how does DCS embedded in a factory. Yokogawa is an industry well known DCS solution pioneer originated in Japan since 1915. Other well known DCS solution provider such as Siemens, Honeywell, ABB, Rockwell, GE, Mitsubishi.
<p align="center">
<img src="/image/brochure_system_structure.JPG" height="90%" width="90%"> 
</p>  

We will only focus on HIS(Human Interface Station), ENG(Engineering Station) and FCS(Field Control Station) in this tutorial. The following system structure is within this tutorial's scope. You will notice OPC(Open Platform Communications) in the structure which is optional to the whole system and only play the role of exchanging data in the automation devices. 
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
            <td align="left">This is a small computer which can take analog or digital input/output and provide computing power (like our PC take keyboard, mouse as input and output to monitor). The communication between FCS and electronics, transmitter(flow, temperature, pressure), or hardwares (motor, actuator, pump) can be wired or wireless. Each input/output will be mapped to a memory address which is important when developing logic and program on ENG. There are two types of input and output (I/O) functions: Process I/O exchanges data with field devices outside FCS; and software I/O is for virtual data exchange within the FCS. FCS can be far away from the physical equipments and located in remote DCS office. 
    </br> </br> In a large plant, you might have multiple FCS, each FCS will be assigned with an IPv4 so that HIS, ENG, OPC can reach out to these FCS via network. FCS is comprised of the following components: Field control unit (FCU, like brain, CPU), Node unit (NU, with communication module), ESB bus/Optical ESB bus (wiring between FCU and NU), House Keeping Unit(HKU). </br></br>
    	<p align="center">
		<img src="/image/FCU_image.JPG" height="60%" width="60%"> 
	        </br>FCU
	</p> 
	<p align="center">
		<img src="/image/FCU_Node_Connect.JPG" height="60%" width="60%"> 
	        </br>FCU and Node Connection
	</p> 
    	    </td>
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

FCS reference: [Reference_1_1_1](https://web-material3.yokogawa.com/TI33K01A12-50E.pdf), [PDF](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/Reference_1_1_1_FCS%20Overview_Integrated%20Production%20Control%20System%20CENTUM%20VPSystem%20Overview.pdf)

## 1.2 Build a DCS Solution
The simplify process to build a DCS solution is as following:
1. Wire the electronics or hardwares from the field or production line into FCS.
2. Use ENG to develop programs and logic and download it to FCS so that FCS can process input and output signal.
3. Use ENG to develop user interface(UI) and download it to HIS operator station so that user can interact with DCS.
4. Use OPC server to collect FCS data on behalf of client program. The data can be stored in SQL database for later processing.

# 2. What You See - Human Interface Station (HIS)
Let's take a look at what a DCS operator would see on their screen on the HIS. You can see browser bar, graphics, some alarm, silo, piping, valve, pumps, and some values. The graphic (piping and instrument diagram) abstract the production process (process flow diagram), equipments, control function, and process real time value. The user interface program directly asks data from FCS or send command to the FCS therefore monitor and control the production process value. ([Reference_2_0_1](https://web-material3.yokogawa.com/TI33J01A11-01EN.pdf), [PDF](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/Reference_2_0_1_HMI%20Overview_TI33J01A11-01EN.pdf))

<p align="center">
<img src="/image/CENTUMVP-HMI.png" height="90%" width="90%"> 
</p>  

## 2.1 Equipment Naming Practice
In a production line, there are large amount of controller, devices and equipments. Usually, we will name each of them into tag name(some alphabet and numbers) which can make the user interface more clean and neat and better management. The nameing convention can be seen in the following ([Reference_2_1_1](https://www.controleng.com/articles/plc-tag-and-address-naming-conventions/), [PDF](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/Control%20Engineering%20_%20PLC%20tag%20and%20address%20naming%20conventions.pdf))

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
We will now explain what shows on the below graphic (see Reference_2_0_1 page 3-7 ~ 3-8)
<p align="center">
<img src="/image/faceplate_view_1.jpg" height="100%" width="100%"> 
	Instrument Faceplate View (click to view larger image)
</p> 

### 2.2.1 PV, SV, MV
Let's first see a simple example. We have an equipment, car(tag name: CAR001). We want to have the car to run at 50 mph (<b>setpoint value, SV</b>) no matter on what kind of surrounding.  Car is stop at the begining without break. By pushing the gas pedal 50%, the real time car speed (<b>process variable, PV</b>) will gradually and quickly start from 0 MPH to 50 MPH, then we can mantain the car speed 50 mph on a flat road with gas pedal 30%. However, when the car encounter a hill, the car will become slower (PV will go down probably to 35 mph) if the gas pedal still at 30%. In order for the car to reach the setpoint value(SV, 50 mph) during the uphill, the loop control will take in charge and press more on the gas pedal (probably 60%) so that we can mantain the car speed to 50 mph.

<p align="center">
<img src="/image/interprete_value_car.jpg" height="70%" width="70%"> 
</p>  

Likewise, on the HIS user interface, you can see tag name represents the equipment. PV indicating the current process variable of the equipment, and the SV indicating the setpoint value from the user. For the car example, the whole tag name for the process variable will become CAR001.PV and the setpoint value will be CAR001.SV. You might also see MV which represent manipulated output value. You can think of MV as the effort for FCS to make PV close to SV, for example 30% or 50% on the gas pedal.

<p align="center">
<table>
    <thead>
        <tr>
            <th align="center">Data Item Name</th>
            <th align="center">Detail</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td align="left">Process Variable (PV)</td>
            <td align="left">The current measured value of a particular part of a process.</td>
        </tr>
        <tr>
            <td align="left">Setpoint Value (SV)</td>
            <td align="left">The desired or target value for an essential variable, or process value of a system.</td>
        </tr>
        <tr>
            <td align="left">Manipulated Output Value (MV)</td>
            <td align="left">The results of control computation processing performed by the control blocks are output as manipulated output values.</td>
        </tr>        
    </tbody>
</table>
</p>

### 2.2.2 AUT, MAN, CAS
These abbrevation indicate control modes ([Reference_2_2_1](https://control.com/textbook/basic-process-control-strategies/cascade-control/#:~:text=Automatic%20mode%3A%20Controller%20automatically%20adjusts,try%20to%20keep%20PV%20%3D%20SP.&text=Cascade%20mode%3A%20Controller%20automatically%20adjusts,by%20primary%20(master)%20controller), [PDF](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/Reference_2_2_1_CH34_Cascade%20Control%20_%20Basic%20Process%20Control%20Strategies%20and%20Control%20System%20Configurations%20_%20Automation%20Textbook.pdf)), ([Reference_2_2_2](https://cr4.globalspec.com/thread/15328/Automatic-Manual-and-Cascade-Control-Mode), [PDF](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/Reference_2_2_2__Automatic%2C%20Manual%2C%20and%20Cascade%20Control%20Mode%20-%20CR4%20Discussion%20Thread.pdf))

<p align="center">
<table>
    <thead>
        <tr>
            <th align="center">Control Mode</th>
            <th align="center">Detail</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td align="center">Manual mode (MAN)</td>
            <td align="left">also called "open-loop control" is where the control element (usually a control valve) is controlled by the operator (a human, usually). He manipulates the control valve and hopes that he can maintain the process at the required setpoint. Eventually he gets tired and switches it over to automatic mode.</td>
        </tr>
        <tr>
            <td align="center">Automatic mode (AUT or AUTO)</td>
            <td align="left">also called "closed-loop control" is where the control element is controlled by the controller (not a human, definitely). It checks the process, compares it to the setpoint and adjusts the control valve to bring the process back to the setpoint. </br></br>Controller automatically adjusts its output to try to keep PV = SP. Setpoint value set “locally” by human operator.</td>
        </tr>
        <tr>
            <td align="center">Cascade mode (CAS)</td>
            <td align="left">Cascade control is not a mode. It's a control scheme like Feedback, Feedforward, and Ratio control. Cascade control always involves two controllers each measuring separate but inter-related processes. One controller is the master and the other is the slave. The master provides the setpoint being used by the slave. </br></br>Controller automatically adjusts its output to try to keep PV = SP. Setpoint value set “remotely” by primary (master) controller.
            <p align="center">
                <img src="/image/cascade_mode_block.JPG" height="50%" width="50%"> 
	        see Reference_2_2_1
            </p> 
            </td>
        </tr>        
    </tbody>
</table>
</p>

# 3. Behind the Scenes - Engineering Station (ENG)
We will discuss the technical side and design logic of the DCS in this section. ([Reference_3_0_1](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/Reference_3_0_1_Centum%20VP_Engineering%20and%20Maintenance%20Training%20Manual.pdf))

## 3.1 Basic User Interface Introduction (video)
Please go the the following Youtube link: [Introduction to FCS Programming with 'Plant in a Box'](https://www.youtube.com/watch?v=BIW77bzX3i0) (1hr) to learn the basic usage of user interface and how to abstract the production process into HIS windows graphic. The following link is the screenshot of the video of each step: [Video Screenshot folder](https://github.com/Dungyichao/OPC_Data_Access/tree/main/image/Youtube%20Tutorial).

## 3.2 Design and Hardware Relationship
Take a look at the following project design level against the physical hardware level.
<p align="center">
<img src="/image/FCS_design_level_5.jpg" height="100%" width="100%"> 
</p>  

Under a project, you can have multiple <b>FCS</b> and multiple <b>HIS</b> based on your existed DCS structure and design. Under a FCS, you can have multiple but limited <b>Node</b>. Under a Node, you can have multiple but limited <b>I/O Module</b> (input/output). 

## 3.3 Project System View
In the following image, you can see the folder tree which represents the hierarchy of the project and its components.
<p align="center">
<img src="/image/Project_System_View.jpg" height="70%" width="70%"> 
	Project System View
</p> 
You can have multiple project folders under system view (red line). Under each project (blue line), you can have multiple FCS and HIS. Under each FCS (green line), you can see IOM folder and FUNCTION_BLOCK folder. Under IOM folder, you can have multiple Node, and each node can contain multiple but limited quantity I/O Module (Analog, Digital, Analog/Digital). Each I/O module contains multiple terminal (physically call channel). There are limited items under FUNCTION_BLOCK folder. In each HIS folder, there is a WINDOW folder, and there are multiple Graphic and Trend folders in the WINDOW folder.

### 3.3.1 FCS and FCU



### 3.3.2 Node and I/O Modules
Input/Output (I/O) and communication modules are mounted on Node unit (NU). And NU transmits those module data to FCS. Some module can handle analog I/O, some can hanle digital I/O, while some can handle both analog and digital I/O. Please refer to [Reference_1_1_1](https://web-material3.yokogawa.com/TI33K01A12-50E.pdf) page 2-9 to see different model of module and their capability. Different node unit can hold different limited amount of I/O module.

Let's say we have a model AAB841 I/O module ([Reference_1_1_1](https://web-material3.yokogawa.com/TI33K01A12-50E.pdf) page 2-9). It is an analog I/O module with 8 channel input and 8 channel output, 1 to 5 V Input, 4 to 20 mA Output. When you add the node module model in the FCS, you will be able to see the terminals (channel) of the I/O module by double click on the node I/O module (shown in the following imgae), 8 terminal for input and the other 8 for output. You can use the Terminal number (such as %Z033101) in function blocks.
<p align="center">
<img src="/image/node_terminal.jpg" height="80%" width="80%"> 
</p> 

For tag name and label, the setting items may vary, depending on Type. For analog I/O, only user-defined label can be set. Tag name cannot be set. For digital I/O, tag name or user-defined label can be set. 

### 3.3.3 Control Function
Function block is a big topic, so we will talk about it in section 3.4.
<p align="center">
<img src="/image/Function_block_overview.JPG" height="80%" width="80%"> 
</p> 

### 3.3.4 Graphic and Trend

## 3.4 Function Block

# 4. How We Collect - OPC Data Access
OPC server will be the one to ask data from each FCS on behalf of us. We will now explain how to write program as OPC client to ask OPC server to retrieve data. ([Link](https://www.programmersought.com/article/51263918194/), [Reference_4_0_1](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/Reference_4_0_1_C%23%20uses%20DA%20and%20UA%20to%20access%20PLC%20through%20KepServer%20-%20Programmer%20Sought.pdf)), ([Link](https://blog.csdn.net/cuoban/article/details/106130764), [Reference_4_0_2](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/Reference_4_0_2_C%23%20OPC%20PLC%20Communication%20Example.pdf))

## 4.1 Connect to OPC Steps
The following is the core steps to communicate with OPC server. You can wrape it inside your program logic. For example, you can first connect to your SQL database to know whatever tag names need to be asked. After asking OPC server and retrieved data, identify the quality of the data, and then insert these data into SQL database. 
<p align="center">
<img src="/image/DCS_OPC_Client_Stpes.jpg" height="40%" width="40%"> 
</p> 

## 4.2 Codes
Create a C# Console Application. Make sure you have the dynamic library which vendor provided to you to access OPC server function. Then, you need to add the library to your program reference.
<p align="center">
<img src="/image/csharp_reference_opcda.JPG" height="80%" width="80%"> 
</p> 

```C#
using OPCAutomation;

//string Connect_str = "Data Source = 172.17.216.5; Initial Catalog=DatabaseName; UID=sa; pwd=some_password; Timeout=10";
//string query_str = "SELECT * FROM TABLE_NAME WHERE 1=1 AND ...."
static void OPC_Collect_Function(string Connect_str, string query_str, string insert_str)
{
    string[] TagWholeName = new string[1000];
    int[] TagCHandles = new int[1000];
    DataTable ReadTagNameTable = new DataTable();    
    ReadTagNameTable = ReadTagName_IP_String(Connect_str, query_str);
    int NumItem = ReadTagNameTable.Rows.Count;
    if (NumItem > 0 && NumItem < 1000)
    {        
        for (int j = 1; j <= NumItem; j++)
        {
            TagWholeName[j] = ReadTagNameTable.Rows[j - 1]["TagName"].ToString();
            TagCHandles[j] = j;
        }
    }
    else
    {
        return;
    }

    OPCServer _OPCServer = new OPCServer();

    _OPCServer.Connect(KEPServerVersion, KEPServerIP);
    _OPCServer.Logon("enguser", "");

    OPCGroups groups;
    OPCGroup group;
    OPCItems items;
    OPCItem item;
    Array strItemIDs;
    Array lClientHandles;
    Array lserverhandles;
    Array lErrors;
    object RequestedDataTypes = null;
    object AccessPaths = null;

    groups = _OPCServer.OPCGroups;
    groups.DefaultGroupIsActive = true; //Set the group collection to be active by default
    groups.DefaultGroupDeadband = 0; //Set dead zone
    groups.DefaultGroupUpdateRate = 200;//Set the update frequency


    group = groups.Add(null);
    group.IsActive = true;
    //group.IsSubscribed = true; //subscribe
    group.UpdateRate = 200;    //Update refresh frequency
    _OPCServer.OPCGroups.DefaultGroupDeadband = 0;
    group.DataChange += new DIOPCGroupEvent_DataChangeEventHandler(KepGroup_DataChange);

    items = group.OPCItems;

    strItemIDs = (Array)TagWholeName;
    lClientHandles = (Array)TagCHandles;
    items.AddItems(NumItem, ref strItemIDs, ref lClientHandles, out lserverhandles, out lErrors, RequestedDataTypes, AccessPaths);

    object qualities = new object(); //opc server will store the quality of the item 
    object timestamps = new object(); //store the timestamp of the read

    Array values; //opc server will store the values in this array
    Array errors; //opc server will store any errors in this array

    //read directly from device
    group.SyncRead((short)OPCDataSource.OPCDevice, NumItem, ref lserverhandles, out values, out errors, out qualities, out timestamps);

    Array qualities_array = (Array)qualities;

    int insert_result = InsertData_IP_String(ReadTagNameTable, values, qualities_array, Connect_str, insert_str);

    groups.RemoveAll();
    _OPCServer.Disconnect();

}
```
Noticed that we only allowed 1000 tag names because we only assign an array to hold 1000 value. You also need to make sure your OPC server allows 1000 data query once. You can also see in the first for loop, we shift the whole table by one row.  


Functions: Connect to database and retrieve tag name information
```C#
using System.Data.SqlClient;

static DataTable ReadTagName_IP_String(string Connect_str, string Select_str)
{
    DataTable tempTable = new DataTable();
    string querystring = Select_str;

    try
    {
        using (SqlConnection Myconn2 = new SqlConnection(Connect_str))
        using (SqlCommand Mycomm2 = new SqlCommand(querystring, Myconn2))
        {
            Myconn2.Open();
            //Mycomm2.Parameters.Add("@CNO", CNO);
            SqlDataAdapter MyAd2 = new SqlDataAdapter();
            MyAd2.SelectCommand = Mycomm2;
            MyAd2.Fill(tempTable);
        }
    }
    catch (Exception e)
    {
        Console.WriteLine(e.ToString());
    }

    return tempTable;
}
```


Functions: Insert to database with retrieved tag name data
```C#
using System.Data.SqlClient;

static int InsertData_IP_String(DataTable dt, Array values, Array Quality, string Connect_str, string insert_str)
{
    string insertstring = insert_str;
    string currentTime = DateTime.Now.ToString("yyyyMMddHHmmss");
    Int16 Code;
    Int16 Quality_Good_Code = 190;
    Int16 Quality_Uncertain_Code = 64;
    using (SqlConnection Myconn2 = new SqlConnection(Connect_str))
    using (SqlCommand Mycomm2 = new SqlCommand(insertstring, Myconn2))
    {

        try
        {
            Myconn2.Open();
            for (int i = 0; i < dt.Rows.Count; i++)
            {

                if (Int16.TryParse(Quality.GetValue(i + 1).ToString(), out Code))
                {
                    if (Code > Quality_Good_Code)
                    {
                        Mycomm2.Parameters.Clear();
                        Mycomm2.Parameters.AddWithValue("@tagname", dt.Rows[i]["TagName"].ToString());
                        Mycomm2.Parameters.AddWithValue("@tagvalue", values.GetValue(i + 1).ToString());
                        Mycomm2.Parameters.AddWithValue("@tagtime", currentTime);
                        Mycomm2.ExecuteNonQuery();
                        string success_string_log = string.Format("TagName: {0} , value: {1} insert successful", dt.Rows[i]["TagName"].ToString(), values.GetValue(i + 1).ToString());
                        Console.WriteLine(success_string_log);
                        log_string_file(success_string_log);
                    }
                    else if (Code >= Quality_Uncertain_Code)
                    {
                        Mycomm2.Parameters.Clear();
                        Mycomm2.Parameters.AddWithValue("@tagname", dt.Rows[i]["TagName"].ToString());
                        Mycomm2.Parameters.AddWithValue("@tagvalue", values.GetValue(i + 1).ToString());
                        Mycomm2.Parameters.AddWithValue("@tagtime", currentTime);
                        Mycomm2.ExecuteNonQuery();
                        string success_string_log = string.Format("TagName: {0} , value: {1} insert successful. Quality is Uncertain. Quality Code: {2}", dt.Rows[i]["TagName"].ToString(), values.GetValue(i + 1).ToString(), Code.ToString());
                        Console.WriteLine(success_string_log);
                    }
                    else
                    {
                        string error_string_log = string.Format("TagName: {0} Quality is bad, cannot read value. Quality Code: {1}", dt.Rows[i]["TagName"].ToString(), Code.ToString());
                        Console.WriteLine(error_string_log);
                    }
                }
                else
                {
                    string error_quality_string_log = string.Format("TagName: {0} Cannot get Quality, skip this value", dt.Rows[i]["TagName"].ToString());
                    Console.WriteLine(error_quality_string_log);
                }

            }
            Myconn2.Close();
            return 0;
        }
        catch (Exception e)
        {
            Console.WriteLine(e.ToString());
            return 1;
        }
    }
}
```
 You will se quality code in the above code. The quality of a given OPC tag is used to represent the validity of the tag's value (in other words, whether or not an OPC client can trust the data). OPC quality is divided into three main categories: Good (generally indicates the data is valid), Bad (generally indicates the data is not valid), or Uncertain (generally indicates the data is speculative in some manner). Each category is further divided into sub-categories; the exact criteria for using a particular sub-category may vary depending on the end protocol and vendor ([link](https://honeywellprocess-community.force.com/opcsupport/s/article/What-are-the-OPC-Quality-Codes), [Reference_4_2_1](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/Reference_4_2_1_What%20are%20the%20OPC%20Quality%20Codes.pdf))

Function
```C#
static void KepGroup_DataChange(int TransactionID, int NumItems, ref Array ClientHandles, ref Array ItemValues, ref Array Qualities, ref Array TimeStamps)
{

    for (int i = 1; i < NumItems + 1; i++)
    {


        if (Convert.ToInt32(ClientHandles.GetValue(i)) == i)
        {
            if (ItemValues.GetValue(i) != null)
            {
                Console.WriteLine(ItemValues.GetValue(i).ToString());
            }
        }

    }


}
```

Function
```C#
static void KepGroup_DataChange(int TransactionID, int NumItems, ref Array ClientHandles, ref Array ItemValues, ref Array Qualities, ref Array TimeStamps)
{
    for (int i = 1; i < NumItems + 1; i++)
    {


        if (Convert.ToInt32(ClientHandles.GetValue(i)) == i)
        {
            if (ItemValues.GetValue(i) != null)
            {
                Console.WriteLine(ItemValues.GetValue(i).ToString());
            }
        }

    }
}
```

## 4.3 Make Console Application to Service
This step is optional. ([Link](https://stackoverflow.com/questions/7764088/net-console-application-as-windows-service), [Reference_4_3_1](https://github.com/Dungyichao/OPC_Data_Access/blob/main/Documents/Reference_4_3_1_c%23%20-%20.NET%20console%20application%20as%20Windows%20service%20-%20Stack%20Overflow.pdf))

### 4.3.1 Convert the App
First step, right click on the console application solution, add new item, select Windows Service.

Second step, right click on the new create ```Service1.cs``` view code. 
```c#
using System.ServiceProcess;
using System.Threading;

namespace Your_Solution_Name
{
    public partial class Your_Service_Name : ServiceBase
    {
        readonly Program _application = new Program();

        static void Main()
        {
            ServiceBase[] servicesToRun = { new Your_Service_Name() };
            Run(servicesToRun);
        }

        public Your_Service_Name()
        {
            InitializeComponent();
        }

        protected override void OnStart(string[] args)
        {
            Thread thread = new Thread(() => _application.Start());
            thread.Start();
        }

        protected override void OnStop()
        {
            Thread thread = new Thread(() => _application.Stop());
            thread.Start();
        }
    }
}
```

Then you should slightly modify the console application program ```Program.cs```. The original ```public void Main(arg[])``` should change to other name so that it will not conflict with the Main function we created in Service.cs file. There is only one Main function can stay in a program. So we change the original Main() to Main_APP() in  ```Program.cs```.
```C#
static void Main_APP(){

   //connect to database
   .....
   
   //connect to OPC server and disconnect
   .....
   
   //insert into database
   ......
}

public void Start()
{
    Main_APP();
}

public void Stop()
{
    Environment.Exit(1);  //console app

}

```

Double click on the Service1.cs, right click on Service1.cs designer page, click on ```Add Installer```, then a ProjectInstaller.cs will be created. You can click on the ProjectInstaller.cs Design page and click on ```serviceInstaller1``` icon and try to add some detail in the Properties such as service display name, service description. These information will be display when you install this service to Windows Services. In the ```serviceProcessInstaller1``` (also in ProjectInstaller.cs Design page), you can change the Account type (LocalSystem, LocalService, NetworkService, User) in Misc. Rebuild the solution. 

### 4.3.2 Register and run the service
Place the solution folder in C drive (contain program solution as well)
From the Start menu, select the Visual Studio <version> directory, then select Developer Command Prompt for VS <version>  and run as Administrator
Key in  
```installutil C:\Solution_Folder\Solution_Name\bin\Debug\Solution_Name.exe```
and press enter. You should see 
```The Commit phase completed successfully. The transacted install has completed```

Finally, go to Services. Right click on Service Name you created in the the solution, click “Start”.


