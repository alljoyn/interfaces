# org.allseen.LSF.LampService version 1


## Theory of Operation
This interface is provided by the LSF Lamp Service to enable the LSF Lamp
Clients to fetch the version of the Lamp Service and also to get/reset a
Lamp Fault.

The interface is not secure.

## Specification

|              |       				|
|--------------|--------------------------------|
| Version      | 1     				|
| Annotation   | org.alljoyn.Bus.Secure = false |

### Properties

#### Version

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The interface version number.

#### LampServiceVersion

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16                                                   |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

The LampService version number.

#### LampFaults

|            |                                                          |
|------------|----------------------------------------------------------|
| Type       | uint16[]                                                 |
| Access     | read-only                                                |
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = false	|

Array of Lamp Faults.

### Methods

#### ClearLampFault (LampFaultCode) -> (LampResponseCode, LampFaultCode)

This method allows the LSF Lamp Clients to clear a Lamp Fault. 

Input arguments: 

  * **LampFaultCode** --- uint16 --- the code associated with a Lamp Fault to be cleared.

Output arguments:

  * **LampResponseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer                                          		|
| 2     | An operation was unexpected at this time                          		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|

  * **LampFaultCode** --- uint16 --- the code associated with a Lamp Fault to be cleared.

## References

  * The XML definition of the [LampService interface](LampService-v1.xml)


