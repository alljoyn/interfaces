# org.allseen.LSF.ControllerService version 1

## Important Note
This interface has been defined prior to the creation of the interface design guidelines for AllSeen.
Many design decisions in this interface do not comply to the guidelines and constitute bad precedent.
Do not use these interfaces as a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki pages](https://wiki.allseenalliance.org/interfacereviewboard).

For a detailed annotation of the interface design guideline violations in this interface, please
visit the [Gerrit submission page](https://git.allseenalliance.org/gerrit/#/c/5877/).

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

This method allows the LSF Controller Clients to reset all the lighting data. The Controller Service persists 
the created Scenes, Lamp Groups, Presets, etc and all of this data is reset (i.e., wiped off) when this API is called.

Input arguments: None

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 15    | The requested operation was only partially successful                         |


#### GetControllerServiceVersion () -> (version)

This method allows the LSF Controller Clients to fetch the version of the Controller Service. This is the same as About SoftwareVersion field.

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


