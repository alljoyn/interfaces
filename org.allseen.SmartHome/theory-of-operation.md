1 Theory of Operation

This document describes the basic theory of operation for the AllJoyn Smart Home Service Framework.

1.1 Overview

The AllJoyn Smart Home Service Framework provides capabilities of centralized management in smart home. It uses three roles of smart home entities, i.e., the smart home server, the smart home controller and the smart home controllee, which interact together to form the centralized management smart home system. 

The smart home controller is an AllJoyn-enabled smart home entity that controls the smart home controllee through the smart home server.

The smart home controllee is an AllJoyn-enabled smart home entity that provides controlling services which are made available to the smart home controller through the smart home server. The role of the smart home server is  used as the centralized enabler providing the basis for the centralized management system. With the smart home server as the centralized enabler, smart home controllees operating inside home network, e.g., home appliances, home environment sensing devices and home security devices can be centrally controlled by the smart home controller.

Figure 1 illustrates the high level structure of the centralized management system.
                                     +----------------------+
									 | Smart Home Controller|
									 +----------+-----------+
									            |
												|
								     +----------+-----------+
									 |   Smart Home Server  |
									 +----------+-----------+
									            |
												|
					-------------+--------------+---------------+--------------
                                 |                              |
                                 |                              |
                     +-----------+-----------+     +------------+------------+
                     |Smart Home Controllee A|     | Smart Home Controllee B |
                     +-----------------------+	   +-------------------------+		 
					Figure 1. The high level structure of the centralized management system

The core functionality of the smart home server is to enable centralized management of the smart home controller and smart home controllees in smart home. The pattern is that the smart home controller and smart home controllees are connected to and managed by the smart home server, where the AllJoyn communication between the smart home controller and smart home controllees is hosted centrally by the smart home server. So the smart home controller is able to control smart home controllees through the smart home server.

Centralized management of the smart home controller and smart home controllees moves more complicated logic and implementation into the smart home server and keeps the smart home controller and smart home controllee simpler and at lower cost.

In addition, the protocol of connecting in smart home can be various, such as Wi-Fi and Bluetooth. In such case, smart home server can be regarded as a bridge to link one smart home device and another. For instance, in Figure 1, by Wi-Fi, the smart home controller can be well connected with the smart home server which also can be linked with smart home controllee A and smart home controllee B through Bluetooth. Therefore, AllJoyn communication between the smart home controller and smart home controllee A and smart home controllee B will be accomplished because of the interrelationships with the smart home server.
					
1.2 Common interaction flows

The AllJoyn Smart Home Service Framework provides both the smart home controllee-facing interface, i.e., ControlleeManager interface between the AllJoyn smart home server and the AllJoyn smart home controllee, and the smart home controller-facing interface, i.e., CentralizedController interface and GroupController interface between the AllJoyn smart home server and the AllJoyn smart home controller.

1.2.1 ControlleeManager interface

To be able to join the centralized management system, a smart home controllee needs to register with the smart home server. This will signal that the smart home controllee is interested to be centralized managed. The information of the smart home server is made known to the smart home controllee based on previous configuration. The configuration of the smart home controllee is out of scope of the present service framework. It is very likely that the smart home controller has knowledge of its local smart home server and therefore can configure the smart home controllee prior to its registration.

Figure 2 shows the interaction flow of this registration procedure.
           Smart Home Controllee                                Smart Home Server
		             |                                                  |
					 |   <-Establish session with smart home server->   |                                      
					 |                                                  |
					 |    1.Invoke RegisterControllee method            |      
					 +------------------------------------------------->|
					 |                                                  |
					 |  2.Invoke IntrospectRemoteObjectAsync method     |
					 |<-------------------------------------------------+
					 |                                                  |
					 |  3.Smart home controllee interface information   |
					 +------------------------------------------------->|
					 |                                                  |
					 |                                              /---+
					 |                                             /    |
					 |  4.Store the interface information and     /     |
					 |  proxy objects for smart home controllee  +      |
					 |  add smart home controllee to device list  \     |
					 |                                             \    |
					 |                                              \-->|
					 |                                                  |
					 |                                                  |
		        Figure 2. The interaction flow of smart home controllee registration
				
1. To register with the smart home server, the smart home controllee invokes the RegisterControllee method of the smart home server. The smart home controllee shall send its wellKnownName, uniqueName, deviceId and objectPaths.
2. Since the smart home server previously knows nothing about the smart home controllee, the smart home server makes a remote Introspect method call to discover the smart home controllee’s capabilities.
3. The smart home controllee returns its interface information to the smart home server.
4. The smart home server stores the interface information and proxy objects for the smart home controllee. The smart home controllee is added to the device list including its deviceId, wellKnownName and uniqueName.

For each successful registration, the smart home server get the interface information of the smart home controllee and completes ProxyBusObject to gain access to the BusObject of the smart home controllee. The smart Home server is responsible for maintaining the list of online smart home controllees, i.e., DeviceList that is determined by the smart home controller using AllJoyn Ping. From the maintained DeviceList, the smart home controller can find out each available smart home controllee that can be centralized controlled by the smart home controller.

To leave the centralized management system, a smart home controllee unregisters with the smart home server by making a UnregisterControllee method call on the smart home server’s bus object.

1.2.2 CentralizedController Interface

Once the smart home controllee successfully registers with the smart home server, the smart home server can make the centralized management of the smart home controllee available for the smart home controller. The smart home server maintains the available smart home controllees as DeviceList through which the smart home controller can discover such smart home controllees. The smart home controller can further discover the smart home controllee’s capabilities and invoke the smart home controllee’s action through the smart home server.

Figure 3 shows the interaction flow of this centralized control procedure.
           Smart Home Controller                             Smart Home Server                               Smart Home Controllee
                     |                                              |                                                  |
                     |<-Establish session with smart home server->  | <-Establish session with smart home server->     |
                     |                                              |    <-Register with smart home server->           |
                     |                                              |                                                  |
                     |  1.Invoke QueryInterfaces method	            |                                                  |
                     +--------------------------------------------->|                                                  |
                     |                                              |                                                  |
                     |2.Smart home controllee interface information |                                                  |
                     |<---------------------------------------------+                                                  |
                     |                                              |                                                  |  
                     |     3.Invoke InvokeDeviceAction method       |                                                  |
                     +--------------------------------------------->|                                                  |
                     |                                              |                                                  |
                     | 4.Response of InvokeDeviceAction method call |                                                  |
                     |<---------------------------------------------+                                                  |
                     |                                              |                                                  |
                     |                                              | 5.Call the smart home controllee's method        |
                     |                                              +------------------------------------------------->|
                     |                                              |                                                  |
                     |                                              |6.Reply of the smart home controllee's method call|
                     |                                              |<-------------------------------------------------+
                     |                                              |                                                  |
                     | 7.Send InvokeDeviceActionReturnValue Signal  |                                                  |
                     |<---------------------------------------------+                                                  |
                     |                                              |                                                  |
                                    Figure 3. The interaction flow of centralized control
1. For the smart home controllee that is of interest to the smart home controller, the smart home controller needs to learn about its capabilities. Since the smart home server already has knowledge of the registered smart home controllee, it is therefore in charge of discovery operation of the smart home controllee’s capabilities. The smart home controller invokes the QueryInterfaces method of the smart home server for the interface information of the smart home controllee. The smart home controllee shall send the deviceId of the smart home controllee.
2. The smart home server returns the interface information of the smart home controllee to the smart home controller.
3. After the smart home controllee’s capabilities have been discovered by the smart home controller, the control process of the smart home controllee can be executed. This kind of process is centralized and it uses the smart home server as a central point of action invoking. The smart home controller invokes the InvokeDeviceAction method of the smart home server. The smart home controller shall send the information of the smart home controllee to be controlled that includes deviceId, objectPath, interfaceName, methodName and input arguments of the called smart home controllee’s method.
4. The smart home server provides the InvokeDeviceAction method reply with invocationID that is to be used to correlate the smart home controllee’s reply with this particular action invocation.
5. The smart home controllee’s method call is handled by the smart home server. Based on the information provided by the InvokeDeviceAction method call, the smart home server calls the method of the target smart home controllee. 
6. When the target smart home controllee successfully executes the desired action, the smart home controllee sends the returned value of the method call to the smart home server.
7. The smart home server sends the InvokeDeviceActionReturnValue  signal to the smart home controller. The returned value of the smart home controllee’s method call is provided to the smart home controller in the InvokeDeviceActionReturnValue signal with the invocationID.

1.2.3 GroupController Interface

Smart home server supports a group management mechanism which can be used to control a group of smart home controllees’ actions.

The smart home controller figures out smart home controllee’s actions suitable for grouping and configures them as the actions group. By grouping desired smart home controllees’ actions together, the smart home controller is capable of controlling bunch of smart home controllees efficiently in smart home.

Figure 4 shows the interaction flow of the group control procedure
  Smart Home Controller                         Smart Home server                           Smart Home Controllee A 	                   Smart Home Controllee B 	
          |                                            |                                                |                                            |
          |<-Establish session with smart home server->|  <-Establish session with smart home server->  |                                            |
          |                                            |       <-Register with smart home server->      |                                            |
          |                                            | <-------------------------Establish session with smart home server---------------------->   |
		  |                                            | <---------------------------Register with smart home server----------------------------->   |
		  |                                            |                                                |                                            |
		  |  1.Invoke DefineGroupAction method         |                                                |                                            |
		  +------------------------------------------->|                                                |                                            |
		  |                                            |                                                |                                            |
		  |                                            +--\ 2.Store desired action information of smart |                                            |
		  |                                            |   \     home controllee A and smart home       |                                            |
          |                                            |   /	 controllee B for the group ,add group  |                                            |
          |                                            |<-/	name and groupDescription to GroupActionList|                                            | 
      	  |                                            |                                                |                                            |
          | 3.Response of DefineGroupAction method call|                                                |                                            |
          |<-------------------------------------------+                                                |                                            |
          |                                            |                                                |                                            |
          |  4.Invoke InvokeGroupAction method         |                                                |                                            |
          +-------------------------------------------->                                                |                                            |
          |                                            |                                                |                                            |
          |5.Response of InvokeGroupAction method call |                                                |                                            |
          |<-------------------------------------------+                                                |                                            |
          |                                            |                                                |                                            |
          |                                            |  6. Call smart home controlle A's method       |                                            |
          |                                            +----------------------------------------------->|                                            |
          |                                            |                                                |                                            |
          |                                            |	                       7. Call smart home controlle B's method                           |
          |                                            +-------------------------------------------------------------------------------------------->|
          |                                            |                                                |                                            |
          |                                            |8.Reply of smart home controllee A's method call|                                            |
          |                                            |<-----------------------------------------------+                                            |
          |                                            |                                                |                                            |
          |9.Send InvokeGroupActionReturnValue signal  |                                                |                                            |
          |<-------------------------------------------+                                                |                                            |
          |                                            |                                                |                                            |
          |                                            |                      10.Reply of smart home controllee B's method call                      |
          |                                            |<--------------------------------------------------------------------------------------------+
          |                                            |                                                |                                            |
          | 11.Send InvokeGroupActionReturnValue signal|                                                |                                            |
          |<-------------------------------------------+                                                |                                            |
          |                                            |                                                |                                            | 
          |                                            |                                                |                                            |
                                                          Figure 4. The interaction flow of group control
1. The smart home controller requests to create a group of smart home controllees’ actions by invoking the DefineGroupAction method of the smart home server. The smart home controller shall provide the information, i.e., groupName, groupDescription and groupActionArgs including desired actions of smart home controllee A and smart home controllee B.
2. The smart home server stores the group for the desired actions of smart home controllee A and smart home controllee B. The information of groupName,and groupDescription are added to GroupList. The GroupList is maintained by the smart home server from which the smart home controller can find out each available group that can be invoked by the smart home controller.
3. The smart home server sends the DefineGroupAction method reply to the smart home controller.
4. Upon successful creation of the actions group, the smart home controller is capable of controlling smart home controllee A and smart home controllee B by issuing a single command. To do this, the smart home controller invokes the InvokeGroupAction  method of the smart home server with the groupName of the desired group.
5. The smart home server sends the InvokeGroupAction  method reply to the smart home controller.
6. The actions invoking of smart home controllee A and smart home controllee B is handled individually by the smart home server. Based on the desired actions defined by the group, the smart home server calls the method of smart home controllee A.
7. The smart home server calls the method of smart home controllee B.
8. When smart home controllee A successfully invokes the action, smart home controllee A sends the returned value of the method call to the smart home server.
9. The smart home server sends the InvokeGroupActionReturnValue  signal to the smart home controller. The returned value of smart home controllee A’s method call is provided to the smart home controller in the InvokeGroupActionReturnValue  signal.
10. When smart home controllee B successfully invokes the action, smart home controllee B sends the returned value of the method call to the smart home server.
11. The smart home server sends the InvokeGroupActionReturnValue  signal to the smart home controller. The returned value of smart home controllee B’s method call is provided to the smart home controller in the InvokeGroupActionReturnValue signal.

To delete the existing group of smart home controllees’ actions, the smart home controller can invokes the DeleteGroupAction method of the smart home server with groupName. This will move the specified actions group from GroupList maintained by the smart home server.
														  
