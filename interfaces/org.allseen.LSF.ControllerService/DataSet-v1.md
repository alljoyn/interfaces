# org.allseen.LSF.ControllerService.DataSet version 1

## Important Note
This interface has been defined prior to the creation of the interface design guidelines for AllSeen.
Many design decisions in this interface do not comply with the guidelines and constitute bad precedent.
Do not use these interfaces as a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki pages](https://wiki.allseenalliance.org/interfacereviewboard).

For a detailed annotation of the interface design guideline violations in this interface, please
visit the [Gerrit submission page](https://git.allseenalliance.org/gerrit/#/c/5631/).


## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to fetch all the Lamp metadata using a single method call. Lamp metadata 
consists of the following:

1. Lamp name and associated language tag
2. Lamp details which are LSF-specific metadata that the lamp exposes such that information 
about the lamp can be queried via a Controller Service.  Lamp Details are read-only 
and set at the time of manufacturing.  
3. Lamp State which is the state information that the Lamp persists through reset or power 
cycle that includes End-User set state attributes such as Hue, Saturation, Color Temperature, 
and Brightness. We need to persist the state to ensure that the Lamp comes back ON at the
same state that it was it before the power cycle/reset.
4. Lamp Parameters which are read-only volatile parameters that are read from the Lamp hardware.



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
| Annotation | org.freedesktop.DBus.Property.EmitsChangedSignal = true	|

The interface version number.

### Methods

#### GetLampDataSet (lampID, language) -> (responseCode, lampID, language, lampName, lampDetails, lampState, lampParameters)

This method allows the LSF Controller Clients to fetch all the Lamp metadata. 

Input arguments:

  * **lampID** --- string --- the unique identifier of the Lamp.
  * **language** --- string --- the language tag associated with the Lamp Name.

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
| 16    | The entity of interest was not found                   			|

  * **lampID** --- string --- the unique identifier of the Lamp.
  * **language** --- string --- the language tag associated with the Lamp Name.
  * **lampName** --- string --- the name of the Lamp. The max supported size is 256 bytes. 
  * **lampDetails** --- key value pairs --- the details associated with the Lamp. The keys are 
    the property names in the [LampDetails interface](/org.allseen.LSF/LampDetails-v1).
  * **lampState** --- key value pairs --- the state of the Lamp. The keys are the property names in the 
    [LampState interface](/org.allseen.LSF/LampState-v1). 
  * **lampParameters** --- key value pairs --- the parameters associated with the Lamp. The keys are the property 
    names in the [LampParameters interface](/org.allseen.LSF/LampParameters-v1).

## References

  * The XML definition of the [DataSet interface](DataSet-v1.xml)

