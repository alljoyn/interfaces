# org.allseen.LSF.ControllerService.DataSet version 1


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
  * **lampDetails** --- key value pairs --- the details associated with the Lamp. The keys are the property names in the [LampDetails interface](LampDetails-v1.xml) interface.
  * **lampState** --- key value pairs --- the state of the Lamp. The keys are the property names in the [LampState interface](LampState-v1.xml) interface. 
  * **lampParameters** --- key value pairs --- the parameters associated with the Lamp. The keys are the property names in the [LampParameters interface](LampParameters-v1.xml) interface.

## References

  * The XML definition of the [DataSet interface](DataSet-v1.xml)

