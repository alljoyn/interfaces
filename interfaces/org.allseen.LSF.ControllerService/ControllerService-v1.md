# org.allseen.LSF.ControllerService version 1


## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to perform system-wide operations on the Controller Service like resetting the
lighting data.

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

### Methods

#### LightingResetControllerService () -> (responseCode)

This method allows the LSF Controller Clients to reset all the lighting data like
groups, scenes, etc. 

Input arguments: None

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 15    | The requested operation was only partially successful                         |


#### GetControllerServiceVersion () -> (version)

This method allows the LSF Controller Clients to fetch the version of the Controller Service. 

Input arguments: None

Output arguments:

  * **version** --- uint16 --- the version of the Controller Service

### Signals

#### ControllerServiceLightingReset -> ()

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The ControllerServiceLightingReset signal is used to notify all the LSF Controller Clients that the lighting data has been reset.

Output arguments: None

## References

  * The XML definition of the [ControllerService interface](ControllerService-v1.xml)


