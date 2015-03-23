3 org.allseen.SmartHome.GroupController version 1

3.1 Specification

|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = false                                        |

3.1.1 Properties

3.1.1.1 Version

|-----------------------|-----------------------------------------------------------------------|
| Type                  | uint16                                                                |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = invalidates |

Interface version number.

3.1.1.2 GroupList

|-----------------------|-----------------------------------------------------------------------|
| Type                  | GroupInfo[]                                                          |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = invalidates |

Array of the groupName and the groupDescription of each smart home controllees actions group.


3.1.2 Methods

3.1.2.1 DefineGroupAction(groupName, groupDescription, groupActionArgs)

Create the group containing a collection of registered smart home controllees with the desired 
actions for smart home controllees.

Input arguments:

  * **groupName** --- string --- The name of smart home controllees actions group
  * **groupDescription** --- string --- The description of smart home controllees actions group
  * **groupActionArgs** --- GroupActionArgs[] --- Array of deviceId, objectPath, interfaceName, methodName, 
	and methodArg in a group

Output arguments:

  * None

Errors raised by this method:

 * org.allseen.SmartHomeService.Error.GroupExist --- Returned if the specified group to be defined already exists
 * org.allseen.SmartHomeService.Error.Invalid --- Returned if a value of input argument was invalid
 * org.allseen.SmartHomeService.Error.Fail --- Returned if a failure of DefineGroupAction operation has occurred
 * org.allseen.SmartHomeService.Error.Null --- Returned if an argument of DefineGroupAction was null

3.1.2.2 InvokeGroupAction(groupName)

Execute Invokes a group of smart home controllees actions identified by the groupName.

Input arguments:

  * **groupName** --- string --- The name of smart home controllees actions group.

Output arguments:

  * None

Errors raised by this method:

 * org.allseen.SmartHomeService.Error.NoSuchGroup --- Returned if the specified group to be invoked does not exist
 * org.allseen.SmartHomeService.Error.Fail --- Returned if a failure of InvokeGroupAction operation has occurred
 * org.allseen.SmartHomeService.Error.Null --- Returned if an argument of InvokeGroupAction was null

3.1.2.3 DeleteGroupAction(groupName)

Delete the group containing a collection of registered smart home controllees with the desired 
actions for smart home controllees.

Input arguments:

  * **groupName** --- string --- The name of smart home controllees actions group.

Output arguments:

  * None

Errors raised by this method:

 * org.allseen.SmartHomeService.Error.NoSuchGroup --- Returned if the specified group to be deleted does not exist
 * org.allseen.SmartHomeService.Error.Fail --- InvokeGroupAction a failure of DeleteGroupActiion has occurred
 * org.allseen.SmartHomeService.Error.Null --- Returned if an argument of DeleteGroupActiion was null

3.1.3 Signals

3.1.3.1 InvokeGroupActionReturnValue -> (groupName, deviceId, methodName, returnStatus, returnValue)

|-----------------------|-----------------------------------|
| Signal Type           | unicast                       |

The way to return the status of the method call and the value generated from the smart home controllee method call.

Output arguments:

  * **groupName** --- string --- textual description of what happened
  * **deviceId** --- string --- the identification of the smart home controllee
  * **methodName** --- string --- the name of the called smart home controllee's method
  * **returnStatus** --- string --- ER_BUS_OBJ_NOT_FOUND is returned if the smart home server cannot find a matching 
	ProxyObject. Otherwise, the status of the smart home controllee method call is returned
  * **returnValue** --- variant --- the value returned from the smart home controllee method call

3.1.4 Named Types

3.1.4.1 struct GroupInfo

Array of the groupName and the groupDescription of each smart home controllees actions group

  * **groupName** --- string --- the name of smart home controllees actions group
  * **groupDescription** --- string ---  the description of smart home controllees actions group

3.1.4.2 struct GroupActionArgs

Array of deviceId, objectPath, interfaceName, methodName, and methodArg in a group

  * **deviceId** --- string --- the identification of the smart home controllee
  * **objectPath** --- string --- objectpath of the interface provided by the smart home controllee
  * **interfaceName** --- string --- the name of the smart home controllee's interface
  * **methodName** --- string --- the name of the called smart home controllee's method
  * **methodArg** --- variant --- the arguments of the smart home controllee's method

3.1.5 Interface Errors

The method calls in this interface use the AllJoyn error message handling feature
(`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error message. The table
below lists the possible errors raised by this interface.

| Error name                                       | Error message                    |
|--------------------------------------------------|----------------------------------|
| org.allseen.SmartHomeService.Error.GroupExist    | Group already exists.            |
| org.allseen.SmartHomeService.Error.Invalid       | A value was invalid.             |
| org.allseen.SmartHomeService.Error.NoSuchGroup   | Group does not exist.            |
| org.allseen.SmartHomeService.Error.Fail          | A failure has occurred.          |
| org.allseen.SmartHomeService.Error.Null          | An argument was null.            |

3.2 References

  * the formal XML definition of the Interface described in this document
	the formal XML definition is in the document "GroupController.xml"

  * the Theory of Operation document
	The document has been split in document "theory-of-operation.md"

  * the interface definition document for related interfaces
	Interfaces in this document are not related to interfaces in other document

