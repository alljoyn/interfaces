# org.allseen.LSF.ControllerService.Lamp version 1

## Important Note
This interface has been defined prior to the creation of the interface design guidelines for AllSeen.
Many design decisions in this interface do not comply with the guidelines and constitute bad precedent.
Do not use these interfaces as a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki pages](https://wiki.allseenalliance.org/interfacereviewboard).

For a detailed annotation of the interface design guideline violations in this interface, please
visit the [Gerrit submission page](https://git.allseenalliance.org/gerrit/#/c/5877/).

## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to access and control the Lamps.

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

#### GetAllLampIDs () -> (responseCode, lampIDs)

This method allows the LSF Controller Clients to fetch all the Lamp IDs. 

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

  * **lampIDs** --- string[] --- the list of all Lamp IDs.

#### GetLampSupportedLanguages (lampID) -> (responseCode, lampID, supportedLanguages)

This method allows the LSF Controller Clients to fetch the language tags supported by a Lamp specified by the lampID. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.

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


  * **lampID** --- string --- the Lamp ID.
  * **supportedLanguages** --- string[] --- the list of the supported language tags. 

#### GetLampManufacturer (lampID, language) -> (responseCode, lampID, language, manufacturer)

This method allows the LSF Controller Clients to fetch the name of the manufacturer of a Lamp specified by the lampID. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
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


  * **lampID** --- string --- the Lamp ID.
  * **language** --- string --- the language tag associated with the Lamp Manufacturer Name.
  * **manufacturer** --- string --- the name of the manufacturer of the Lamp. The max supported size is 256 bytes.

#### GetLampName (lampID, language) -> (responseCode, lampID, language, lampName)

This method allows the LSF Controller Clients to fetch the name of a Lamp specified by the lampID. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
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


  * **lampID** --- string --- the Lamp ID.
  * **language** --- string --- the language tag associated with the Lamp Name.
  * **lampName** --- string --- the name of the Lamp. The max supported size is 256 bytes. 

#### SetLampName (lampID, lampName, language) -> (responseCode, lampID, language)

This method allows the LSF Controller Clients to set the name of a Lamp specified by the lampID. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
  * **lampName** --- string --- the name of the Lamp. The max supported size is 256 bytes.
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
| 12    | The name is empty                          					|
| 16    | The entity of interest was not found                   			|


  * **lampID** --- string --- the Lamp ID.
  * **language** --- string --- the language tag associated with the Lamp Name. 

#### GetLampDetails (lampID) -> (responseCode, lampID, lampDetails)

This method allows the LSF Controller Clients to fetch the details of a Lamp. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.

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


  * **lampID** --- string --- the Lamp ID.
  * **lampDetails** --- key value pairs --- the Lamp Details. The keys are the property names 
                                            in the [LampDetails interface](LampDetails-v1.xml) interface.

#### GetLampParameters (lampID) -> (responseCode, lampID, lampParameters)

This method allows the LSF Controller Clients to fetch the parameters of a Lamp. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.

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


  * **lampID** --- string --- the Lamp ID.
  * **lampParameters** --- key value pairs --- the Lamp Parameters. The keys are the property names 
                                               in the [LampParameters interface](/org.allseen.LSF/LampParameters-v1).

#### GetLampParametersField (lampID, lampParameterFieldName) -> (responseCode, lampID, lampParameterFieldName, lampParameterFieldValue)

This method allows the LSF Controller Clients to fetch one of the parameters of a Lamp. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
  * **lampParameterFieldName** --- string --- the name of the Lamp Parameter field. This should be one of the parameter names in the [LampParameters interface](/org.allseen.LSF/LampParameters-v1).

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
| 9     | Invalid param/state field                                         		|
| 16    | The entity of interest was not found                   			|

  * **lampID** --- string --- the Lamp ID.
  * **lampParameterFieldName** --- string --- the name of the Lamp Parameter field. This should be one of the parameter names in the [LampParameters interface](/org.allseen.LSF/LampParameters-v1).
  * **lampParameterFieldValue** --- variant --- the value of the Lamp Parameter field.

#### GetLampState (lampID) -> (responseCode, lampID, lampState)

This method allows the LSF Controller Clients to fetch the state of a Lamp. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.

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


  * **lampID** --- string --- the Lamp ID.
  * **lampState** --- key value pairs --- the Lamp State. The keys are the property names 
    in the [LampState interface](/org.allseen.LSF/LampState-v1).

#### GetLampStateField (lampID, lampStateFieldName) -> (responseCode, lampID, lampStateFieldName, lampStateFieldValue)

This method allows the LSF Controller Clients to fetch one of the state fields of a Lamp. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
  * **lampStateFieldName** --- string --- the name of the Lamp State field. This should be one of the parameter names in the [LampState interface](/org.allseen.LSF/LampState-v1).

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
| 9     | Invalid param/state field                                         		|
| 16    | The entity of interest was not found                   			|


  * **lampID** --- string --- the Lamp ID.
  * **lampStateFieldName** --- string --- the name of the Lamp State field. This should be one of the parameter names in the [LampState interface](/org.allseen.LSF/LampState-v1).
  * **lampStateFieldValue** --- variant --- the value of the Lamp State field.

#### TransitionLampState (lampID, lampState, transitionPeriod) -> (responseCode, lampID)

This method allows the LSF Controller Clients to transition the Lamp State. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
  * **lampState** --- key value pairs --- the Lamp State. The keys are the property names 
    in the [LampState interface](/org.allseen.LSF/LampState-v1).
  * **transitionPeriod** --- uint16 --- the transition period in milliseconds.

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
| 8     | Value provided was out of range                                   		|
| 16    | The entity of interest was not found                   			|

  * **lampID** --- string --- the Lamp ID.

#### PulseLampWithState (lampID, fromLampState, toLampState, period, duration, numPulses) -> (responseCode, lampID)

This method allows the LSF Controller Clients to apply a Pulse Effect on a Lamp. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
  * **fromLampState** --- key value pairs --- the fromState of the Pulse Effect. The keys are the property names 
    in the [LampState interface](/org.allseen.LSF/LampState-v1).
  * **toLampState** --- key value pairs --- the toState of the Pulse Effect. The keys are the property names 
    in the [LampState interface](/org.allseen.LSF/LampState-v1).
  * **period** --- uint16 --- the Pulse period in milliseconds.
  * **duration** --- uint16 --- the Pulse duration in milliseconds.
  * **numPulses** --- uint16 --- the number of Pulses.

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
| 8     | Value provided was out of range                                   		|
| 16    | The entity of interest was not found                   			|


  * **lampID** --- string --- the Lamp ID.

#### PulseLampWithPreset (lampID, fromPresetID, toPresetID, period, duration, numPulses) -> (responseCode, lampID)

This method allows the LSF Controller Clients to apply a Pulse Effect on a Lamp. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
  * **fromPresetID** --- string --- the fromState of the Pulse Effect specified using a Preset ID.
  * **toPresetID** --- string --- the toState of the Pulse Effect specified using a Preset ID.
  * **period** --- uint16 --- the Pulse period in milliseconds.
  * **duration** --- uint16 --- the Pulse duration in milliseconds.
  * **numPulses** --- uint16 --- the number of Pulses.

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
| 8     | Value provided was out of range                                   		|
| 16    | The entity of interest was not found                   			|


  * **lampID** --- string --- the Lamp ID.

#### TransitionLampStateToPreset (lampID, presetID, transitionPeriod) -> (responseCode, lampID)

This method allows the LSF Controller Clients to transition to the Lamp State specified by the presetID. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
  * **presetID** --- string --- the Preset ID.
  * **transitionPeriod** --- uint16 --- the transition period in milliseconds.

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
| 8     | Value provided was out of range                                   		|
| 16    | The entity of interest was not found                   			|


  * **lampID** --- string --- the Lamp ID.

#### TransitionLampStateField (lampID, lampStateFieldName, lampStateFieldValue, transitionPeriod) -> (responseCode, lampID, lampStateFieldName)

This method allows the LSF Controller Clients to fetch one of the state fields of a Lamp. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
  * **lampStateFieldName** --- string --- the name of the Lamp State field. This should be one of the parameter names in the [LampState interface](/org.allseen.LSF/LampState-v1).
  * **lampStateFieldValue** --- variant --- the value of the Lamp State field.
  * **transitionPeriod** --- uint16 --- the transition period in milliseconds.

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
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 16    | The entity of interest was not found                   			|


  * **lampID** --- string --- the Lamp ID.
  * **lampStateFieldName** --- string --- the name of the Lamp State field. This should be one of the parameter names in the [LampState interface](/org.allseen.LSF/LampState-v1).

#### ResetLampState (lampID) -> (responseCode, lampID)

This method allows the LSF Controller Clients to reset the Lamp State. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.

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
| 8     | Value provided was out of range                                   		|
| 16    | The entity of interest was not found                   			|


  * **lampID** --- string --- the Lamp ID.

#### ResetLampStateField (lampID, lampStateFieldName) -> (responseCode, lampID, lampStateFieldName)

This method allows the LSF Controller Clients to reset a Lamp State field. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
  * **lampStateFieldName** --- string --- the name of the Lamp State field. This should be one of the parameter names in the [LampState interface](/org.allseen.LSF/LampState-v1).

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
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 16    | The entity of interest was not found                   			|


  * **lampID** --- string --- the Lamp ID.
  * **lampStateFieldName** --- string --- the name of the Lamp State field. This should be one of the parameter names in the [LampState interface](/org.allseen.LSF/LampState-v1).

#### GetLampFaults (lampID) -> (responseCode, lampID, lampFaults)

This method allows the LSF Controller Clients to fetch the faults from the Lamp. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.

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


  * **lampID** --- string --- the Lamp ID.
  * **lampFaults** --- uint16[] --- the list of Lamp Faults.   

#### ClearLampFault (lampID, lampFault) -> (responseCode, lampID, lampFault)

This method allows the LSF Controller Clients to reset a Lamp Fault. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.
  * **lampFault** --- uint16 --- the Lamp Fault to clear.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 16    | The entity of interest was not found                   			|

  * **lampID** --- string --- the Lamp ID.
  * **lampFault** --- uint16 --- the Lamp Fault to clear.

#### GetLampServiceVersion (lampID) -> (responseCode, lampID, lampServiceVersion)

This method allows the LSF Controller Clients to get the version of the Lamp Service. 

Input arguments:

  * **lampID** --- string --- the Lamp ID.

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


  * **lampID** --- string --- the Lamp ID.
  * **lampServiceVersion** --- uint16 --- the Lamp Service version.

### Signals

#### LampNameChanged -> (lampID, lampName)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The LampNameChanged signal is used to notify the new name of the Lamp after it is changed.

Output arguments:

  * **lampID** --- string --- the Lamp ID.
  * **lampName** --- string --- the Lamp Name. The max supported size is 256 bytes. 

#### LampStateChanged -> (lampID, lampState)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The LampStateChanged signal is used to notify the new state of the Lamp after it is changed.

Output arguments:

  * **lampID** --- string --- the Lamp ID.
  * **lampState** --- key value pairs --- the Lamp State. The keys are the property names 
    in the [LampState interface](/org.allseen.LSF/LampState-v1).

#### LampsFound -> (lampIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The LampsFound signal is used to notify the list of new Lamp IDs that have 
been found.

Output arguments:

  * **lampIDs** --- string[] --- the list of Lamp IDs of the new Lamps that have been found.

#### LampsLost -> (lampIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The LampsLost signal is used to notify the list of the Lamp IDs that have 
been lost. It means that the Controller Service had discovered the lamp and 
had set up a session with the lamp but the session got lost. When this happens, 
the controller sends this signal to all the LSF controller clients. 

Output arguments:

  * **lampIDs** --- string[] --- the list of Lamp IDs that have been lost.

## References

  * The XML definition of the [Lamp interface](Lamp-v1.xml)


