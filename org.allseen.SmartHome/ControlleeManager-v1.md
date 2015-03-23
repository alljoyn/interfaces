1 org.allseen.SmartHome.ControlleeManager version 1

1.1 Specification

|-----------------------|-----------------------------------------------------------------------|
| Version               | 1                                                                     |
| Annotation            | org.alljoyn.Bus.Secure = false                                        |

1.1.1 Properties

1.1.1.1 Version

|-----------------------|-----------------------------------------------------------------------|
| Type                  | uint16                                                                |
| Access                | read-only                                                             |
| Annotation            | org.freedesktop.DBus.PropertyChanged.EmitsChangedSignal = invalidates |

Interface version number



1.1.2 Methods

1.1.2.1 RegisterControllee(wellKnownName, uniqueName, deviceId, objectPaths)

The smart home controllee joins the centralized management system by 
registering with the smart home server.

Input arguments:

  * **wellKnownName** --- String --- Well-known name provided by the smart 
	home controllee
  * **uniqueName** --- String --- Unique name of the smart home controllee
  * **deviceId** --- String --- The identification of the smart home controllee	
	(its format is complied with the deviceId defined in the AllJoyn About interface)
  * **objectPaths** --- String[] --- Objectpaths of the interface provided by 
	the smart home controllee

Output arguments:

  * None

Errors raised by this method:

 * org.allseen.SmartHomeService.Error.DeviceExists --- Returned if the registering 
	smart home controllee already exists
 * org.allseen.SmartHomeService.Error.Invalid --- Returned if a value of input 
	argument was invalid
 * org.allseen.SmartHomeService.Error.IntrospectFail --- Returned if a failure 
	of introspection has occurred
 * org.allseen.SmartHomeService.Error.Fail --- Returned if a failure of 
	RegisterControllee operation has occurred
 * org.allseen.SmartHomeService.Error.Null --- Returned if an argument of 
	RegisterControllee was null

1.1.2.2 UnregisterControllee(deviceId)

The smart home controllee leaves the centralized management system by 
unregistering with the smart home server.

Input arguments:

  * **deviceId** --- String --- The identification of the smart home controllee 
	(its format is complied with the deviceId defined in the AllJoyn About interface).

Output arguments:

  * None

Errors raised by this method:

 * org.allseen.SmartHomeService.Error.NoSuchDevice --- Returned if the 
	unregistering smart home controllee does not exist
 * org.allseen.SmartHomeService.Error.Fail --- Returned if a failure 
	of UnregisterControllee operation has occurred
 * org.allseen.SmartHomeService.Error.Null --- Returned if an argument 
	of UnregisterControllee was null

1.1.3 Interface Errors

The method calls in this interface use the AllJoyn error message handling feature
(`ER_BUS_REPLY_IS_ERROR_MESSAGE`) to set the error name and error message. The table
below lists the possible errors raised by this interface.

| Error name                                         | Error message                             |
|----------------------------------------------------|-------------------------------------------|
| org.allseen.SmartHomeService.Error.DeviceExists    | Device already exists.                    |
| org.allseen.SmartHomeService.Error.Invalid         | A value was invalid.                      |
| org.allseen.SmartHomeService.Error.IntrospectFail  | A failure of introspection has occurred.  |
| org.allseen.SmartHomeService.Error.Fail            | A failure has occurred.                   |
| org.allseen.SmartHomeService.Error.Null            | An argument was null.                     |
| org.allseen.SmartHomeService.Error.NoSuchDevice    | Device does not exist.                    |

1.2 References

  * the formal XML definition of the Interface 
	the formal XML definition is in the documents "ControlleeManager.xml"

  * the Theory of Operation document
	The document has been split in document "theory-of-operation.md"

  * the interface definition document for related interfaces 
	Interfaces in this document are not related to interfaces in other document
