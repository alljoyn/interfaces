# org.allseen.LSF.ControllerService.PulseEffect version 1

## Important Note
This interface has been defined prior to the creation of the interface design guidelines for AllSeen.
Many design decisions in this interface do not comply to the guidelines and constitute bad precedent.
Do not use these interfaces as a template to define your own: they will not pass IRB review.

The latest version of the IRB guidelines can be found on the
[IRB wiki pages](https://wiki.allseenalliance.org/interfacereviewboard).

For a detailed annotation of the interface design guideline violations in this interface, please
visit the [Gerrit submission page](https://git.allseenalliance.org/gerrit/#/c/5631/).


## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to create, update, delete, look up and apply Pulse Effects on Lamps.

The Pulse Effect brightens and then dims a Lamp or Lamp Group set to a predefined hue, 
saturation, or color temperature at a defined duty cycle (ratio between pulse duration 
in milliseconds and period in milliseconds).

The Controller Service can store only a certian maximum number of Pulse Effects or until the size of the 
persisted Pulse Effects file reaches a certian specified value whichever occurs first. If the user attempts to create more 
than the allowed number of Pulse Effects, "no slot for new entry" is returned as the responseCode or if the user tries to
create a new Pulse Effect after the size of the Pulse Effect file reaches the maximum allowed value, "Not enough resources" is
returned as the responseCode. These maximum values are dictated by the product that implements this interface.

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

#### GetAllPulseEffectIDs () -> (responseCode, pulseEffectIDs)

This method allows the LSF Controller Clients to fetch all the Pulse Effect IDs. 

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

  * **pulseEffectIDs** --- string[] --- the list of all Pulse Effect IDs.

#### GetPulseEffectName (pulseEffectID, language) -> (responseCode, pulseEffectID, language, pulseEffectName)

This method allows the LSF Controller Clients to fetch the name of a Pulse Effect specified by the pulseEffectID. 

Input arguments:

  * **pulseEffectID** --- string --- the Pulse Effect ID.
  * **language** --- string --- the language tag associated with the Pulse Effect Name.

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

  * **pulseEffectID** --- string --- the Pulse Effect ID.
  * **language** --- string --- the language tag associated with the Pulse Effect Name.
  * **pulseEffectName** --- string --- the name of the Pulse Effect. The max supported size is 256 bytes. 

#### SetPulseEffectName (pulseEffectID, pulseEffectName, language) -> (responseCode, pulseEffectID, language)

This method allows the LSF Controller Clients to set the name of a Pulse Effect specified by the pulseEffectID. 

Input arguments:

  * **pulseEffectID** --- string --- the Pulse Effect ID.
  * **pulseEffectName** --- string --- the name of the Pulse Effect. The max supported size is 256 bytes.
  * **language** --- string --- the language tag associated with the Pulse Effect Name.

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


  * **pulseEffectID** --- string --- the Pulse Effect ID.
  * **language** --- string --- the language tag associated with the Pulse Effect Name. 

#### CreatePulseEffect (toLampState, pulsePeriod, pulseDuration, numPulses, fromLampState, toPresetID, fromPresetID, pulseEffectName, language) -> (responseCode, pulseEffectID)

This method allows the LSF Controller Clients to create a Pulse Effect. 

Input arguments:

  * **toLampState** --- key value pairs --- the toState of the Pulse Effect. The keys are the property names 
    in the [LampState interface](/org.allseen.LSF/LampState-v1).
  * **pulsePeriod** --- uint16 --- the Pulse period in milliseconds.
  * **pulseDuration** --- uint16 --- the Pulse duration.
  * **numPulses** --- uint16 --- the number of Pulses.
  * **fromLampState** --- key value pairs --- the fromState of the Pulse Effect. The keys are the property names 
    in the [LampState interface](/org.allseen.LSF/LampState-v1).
  * **toPresetID** --- string --- the toState of the Pulse Effect specified using a Preset ID.
  * **fromPresetID** --- string --- the fromState of the Pulse Effect specified using a Preset ID.
  * **pulseEffectName** --- string --- the name of the Pulse Effect. The max supported size is 256 bytes.
  * **language** --- string --- the language tag associated with the Pulse Effect Name.

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
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 17    | There is no slot for new entry                                              	|

  * **pulseEffectID** --- string --- the ID of the created Pulse Effect.

#### UpdatePulseEffect (pulseEffectID, toLampState, pulsePeriod, pulseDuration, numPulses, fromLampState, toPresetID, fromPresetID) -> (responseCode, pulseEffectID)

This method allows the LSF Controller Clients to update a Pulse Effect. 

Input arguments:

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.
  * **toLampState** --- key value pairs --- the toState of the Pulse Effect. The keys are the property names 
    in the [LampState interface](/org.allseen.LSF/LampState-v1).
  * **pulsePeriod** --- uint16 --- the Pulse period in milliseconds.
  * **pulseDuration** --- uint16 --- the Pulse duration.
  * **numPulses** --- uint16 --- the number of Pulses.
  * **fromLampState** --- key value pairs --- the fromState of the Pulse Effect. The keys are the property names 
    in the [LampState interface](/org.allseen.LSF/LampState-v1).
  * **toPresetID** --- string --- the toState of the Pulse Effect specified using a Preset ID.
  * **fromPresetID** --- string --- the fromState of the Pulse Effect specified using a Preset ID.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer encountered internally.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 11    | The arguments were invalid                                        		|
| 13    | Not enough resources                                               		|
| 16    | The entity of interest was not found                   			|

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.

#### DeletePulseEffect (pulseEffectID) -> (responseCode, pulseEffectID)

This method allows the LSF Controller Clients to delete a Pulse Effect. 

Input arguments:

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer encountered internally                                |
| 2     | An operation was unexpected at this time                          		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 16    | The entity of interest was not found                   			|
| 18    | There is a dependency of the entity for which a delete request was received   |

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.

#### GetPulseEffect (pulseEffectID) -> (responseCode, pulseEffectID, toLampState, pulsePeriod, pulseDuration, numPulses, fromLampState, toPresetID, fromPresetID)

This method allows the LSF Controller Clients to fetch a Pulse Effect. 

Input arguments:

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.

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

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.
  * **toLampState** --- key value pairs --- the toState of the Pulse Effect. The keys are the property names 
    in the [LampState interface](/org.allseen.LSF/LampState-v1).
  * **pulsePeriod** --- uint16 --- the Pulse period in milliseconds.
  * **pulseDuration** --- uint16 --- the Pulse duration.
  * **numPulses** --- uint16 --- the number of Pulses.
  * **fromLampState** --- key value pairs --- the fromState of the Pulse Effect. The keys are the property names 
    in the [LampState interface](/org.allseen.LSF/LampState-v1).
  * **toPresetID** --- string --- the toState of the Pulse Effect specified using a Preset ID.
  * **fromPresetID** --- string --- the fromState of the Pulse Effect specified using a Preset ID.

#### ApplyPulseEffectOnLamps (pulseEffectID, lampIDs) -> (responseCode, pulseEffectID, lampIDs)

This method allows the LSF Controller Clients to apply a Pulse Effect on a set of Lamps. 

Input arguments:

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.
  * **lampIDs** --- string[] --- the list of Lamp IDs.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer encountered internally.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.
  * **lampIDs** --- string[] --- the list of Lamp IDs.

#### ApplyPulseEffectOnLampGroups (pulseEffectID, lampGroupIDs) -> (responseCode, pulseEffectID, lampGroupIDs)

This method allows the LSF Controller Clients to apply a Pulse Effect on a set of LampGroups.

Input arguments:

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.
  * **lampGroupIDs** --- string[] --- the list of LampGroup IDs.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer encountered internally.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.
  * **lampGroupIDs** --- string[] --- the list of LampGroup IDs.

### Signals

#### PulseEffectsNameChanged -> (pulseEffectIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The PulseEffectsNameChanged signal is used to notify the list of the Pulse Effect IDs whose
names have changed.

Output arguments:

  * **pulseEffectIDs** --- string[] --- the list of Pulse Effect IDs.

#### PulseEffectsCreated -> (pulseEffectIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The PulseEffectsCreated signal is used to notify the list of the Pulse Effect IDs that have 
been created.

Output arguments:

  * **pulseEffectIDs** --- string[] --- the list of Pulse Effect IDs that have been created.

#### PulseEffectsUpdated -> (pulseEffectIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The PulseEffectsUpdated signal is used to notify the list of the Pulse Effect IDs that have 
been updated.

Output arguments:

  * **pulseEffectIDs** --- string[] --- the list of Pulse Effect IDs that have been updated.

#### PulseEffectsDeleted -> (pulseEffectIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The PulseEffectsDeleted signal is used to notify the list of the Pulse Effect IDs that have 
been deleted.

Output arguments:

  * **pulseEffectIDs** --- string[] --- the list of Pulse Effect IDs that have been deleted.

## References

  * The XML definition of the [PulseEffect interface](PulseEffect-v1.xml)

