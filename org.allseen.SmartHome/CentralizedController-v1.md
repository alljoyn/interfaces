2 org.allseen.SmartHome.CentralizedController version 1



2.1 Specification

|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = false                                        |

2.1.1 Properties

2.1.1.1 Version

|-----------------------|-----------------------------------------------------------------------|
| Type                  | uint16                                                                |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = invalidates |
                                
Interface version number.

2.1.1.2 DeviceList

|-----------------------|-----------------------------------------------------------------------|
| Type                  | DeviceInfo[]                                                          |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = invalidates |

The list of online smart home controllees detected by the smart home server using the AllJoyn ping() mechanism.



2.1.2 Methods

2.1.2.1 QueryInterfaces(deviceId) -> (interfaceInfo) 


    Provide the mechanism for the smart home controller to query the interface information of the smart home controllee
    registered on the smart home server.

Input arguments:

  * **deviceId** --- string --- The identification of the smart home controllee (its format is complied with the deviceId defined in the AllJoyn About interface).

Output arguments:

  * **interfaceInfo**  --- InterfaceInfo[] --- Smart home controllee interface information.
	                              

Errors raised by this method:

 * org.allseen.SmartHomeService.Error.NoSuchDevice --- Returned if the specified smart home controllee to be queried does not exist
 * org.allseen.SmartHomeService.Error.Fail --- Returned if a failure of QueryInterfaces operation has occurred
 * org.allseen.SmartHomeService.Error.Null --- Returned if an argument of QueryInterfaces was null


2.1.2.2 InvokeDeviceAction(deviceId, objectPath, interfaceName, methodName, arguments) -> (invocationId)

    Provide the mechanism for the smart home controller to call the methods provided by the smart home controllee 
	through the smart home server.

Input arguments:

  * **deviceId** --- string --- The identification of the smart home controllee (its format is complied with the deviceId defined in the AllJoyn About interface).
  * **objectPath** --- string --- Objectpath of the interface provided by the smart home controllee
  * **interfaceName** --- string --- Name of the called interface
  * **methodName** --- string --- Name of the called method.
  * **arguments** --- variant --- The input parameters of the called method.

Output arguments:

  * **invocationId**  --- uint32 --- The identification of the InvokeDeviceAction operation


Errors raised by this method:
 
  * org.allseen.SmartHomeService.Error.Invalid --- Returned if a value of input argument was invalid
  * org.allseen.SmartHomeService.Error.Null --- Returned if an argument of InvokeDeviceAction was null


2.1.3 Signals

2.1.3.1 InvokeDeviceActionReturnValue -> (methodName,returnStatus ,returnValue,invocationId)

|-----------------------|-----------------------------------|
| Signal Type           | unicast                           |  


The way to return the status of the smart home controllee method call and the replied value generated from the smart home controllee method call.


Output arguments:

  * **methodName** --- string --- the name of the called smart home controllee’s method.
  * **returnStatus** --- string --- ER_BUS_OBJ_NOT_FOUND is returned if the smart home server cannot find a matching ProxyObject.
                                    Otherwise, the status of the smart home controllee method call is returned.
  * **returnValue** --- vatiant --- the value returned from the smart home controllee method call.
  * **invocationId** --- uint32 --- the identification of the InvokeDeviceAction operation


2.1.4 Named Types

2.1.4.1 struct DeviceInfo
    
	The list of online smart home controllees detected by the smart home server using the AllJoyn ping() mechanism.
    Array of deviceId, wellKnownName and uniqueName.

	* **deviceId** --- string --- the identification of the smart home controllee.
	* **wellknownName** --- string --- the well-known name provided by the smart home controllee.
	* **uniqueName** --- string --- the unique name of the smart home controllee

2.1.4.2 struct InterfaceInfo

    Smart home controllee interface information, i.e., array of objectPath and interfaceName
	
    * **objectPath** --- string --- objectpath of the interface provided by the smart home controllee
	* **interfaceName** --- string[] --- Name of the smart home controllee interface


2.1.5 Interface Errors

The method calls in this interface use the AllJoyn error message handling feature
(`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error message. The table
below lists the possible errors raised by this interface.

| Error name                                         | Error message                             |
|----------------------------------------------------|-------------------------------------------|
| org.allseen.SmartHomeService.Error.Invalid         | A value was invalid.                      |
| org.allseen.SmartHomeService.Error.Fail            | A failure has occurred.                   |
| org.allseen.SmartHomeService.Error.Null            | An argument was null.                     |
| org.allseen.SmartHomeService.Error.NoSuchDevice    | Device does not exist.                    |


2.2 References

In this section, list references for the Interface definition (standards, RFCs, related interfaces, etc.).
Make sure to include at least the following references:

  * the formal XML definition of the Interface
	the formal XML definition is in the document "CentralizedController.xml"

  * the Theory of Operation document
  The document has been split in document "theory-of-operation.md"

  * the interface definition document for related interfaces 
  Interfaces in this document are not related to interfaces in other document
