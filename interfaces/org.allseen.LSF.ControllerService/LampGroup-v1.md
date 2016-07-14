# org.allseen.LSF.ControllerService.LampGroup version 1

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
Clients to create, update, delete, look up and control the state of LampGroups.

A LampGroup is a logical grouping of Lamps allowing them to be controlled simultaneously 
as if they are a single Lamp. LampGroups can contain other LampGroups i.e., nesting is
supported. 

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

#### GetAllLampGroupIDs () -> (responseCode, lampGroupIDs)

This method allows the LSF Controller Clients to fetch all the LampGroup IDs. 

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


  * **lampGroupIDs** --- string[] --- the list of all LampGroup IDs.

#### GetLampGroupName (lampGroupID, language) -> (responseCode, lampGroupID, language, lampGroupName)

This method allows the LSF Controller Clients to fetch the name of a LampGroup specified by the lampGroupID. 

Input arguments:

  * **lampGroupID** --- string --- the LampGroup ID.
  * **language** --- string --- the language tag associated with the LampGroup Name.

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


  * **lampGroupID** --- string --- the LampGroup ID.
  * **language** --- string --- the language tag associated with the LampGroup Name.
  * **lampGroupName** --- string --- the name of the LampGroup. The max supported size is 256 bytes. 

#### SetLampGroupName (lampGroupID, lampGroupName, language) -> (responseCode, lampGroupID, language)

This method allows the LSF Controller Clients to set the name of a LampGroup specified by the lampGroupID. 

Input arguments:

  * **lampGroupID** --- string --- the LampGroup ID.
  * **lampGroupName** --- string --- the name of the LampGroup. The max supported size is 256 bytes.
  * **language** --- string --- the language tag associated with the LampGroup Name.

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



  * **lampGroupID** --- string --- the LampGroup ID.
  * **language** --- string --- the language tag associated with the LampGroup Name. 

#### CreateLampGroup (lampIDs, lampGroupIDs, lampGroupName, language) -> (responseCode, lampGroupID)

This method allows the LSF Controller Clients to create a LampGroup. 

Input arguments:

  * **lampIDs** --- string[] --- Array of Lamp IDs.
  * **lampGroupIDs** --- string[] --- Array of LampGroup IDs.
  * **lampGroupName** --- string --- the LampGroup Name. The max supported size is 256 bytes. 
  * **language** --- string --- the language tag associated with the LampGroup Name.

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


  * **lampGroupID** --- string --- the ID of the created LampGroup.

#### UpdateLampGroup (lampGroupID, lampIDs, lampGroupIDs) -> (responseCode, lampGroupID)

This method allows the LSF Controller Clients to update a LampGroup. 

Input arguments:

  * **lampGroupID** --- string --- the ID of the LampGroup.
  * **lampIDs** --- string[] --- Array of Lamp IDs.
  * **lampGroupIDs** --- string[] --- Array of LampGroup IDs.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 11    | The arguments were invalid                                        		|
| 13    | Not enough resources                                               		|
| 16    | The entity of interest was not found                   			|


  * **lampGroupID** --- string --- the ID of the LampGroup.

#### DeleteLampGroup (lampGroupID) -> (responseCode, lampGroupID)

This method allows the LSF Controller Clients to delete a LampGroup. 

Input arguments:

  * **lampGroupID** --- string --- the ID of the LampGroup.

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


  * **lampGroupID** --- string --- the ID of the LampGroup.

#### GetLampGroup (lampGroupID) -> (responseCode, lampGroupID, lampIDs, lampGroupIDs)

This method allows the LSF Controller Clients to fetch a LampGroup. 

Input arguments:

  * **lampGroupID** --- string --- the ID of the LampGroup.

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


  * **lampGroupID** --- string --- the ID of the LampGroup.
  * **lampIDs** --- string[] --- Array of Lamp IDs.
  * **lampGroupIDs** --- string[] --- Array of LampGroup IDs.

#### TransitionLampGroupState (lampGroupID, lampState, transitionPeriod) -> (responseCode, lampGroupID)

This method allows the LSF Controller Clients to transition the LampGroup State. 

Input arguments:

  * **lampGroupID** --- string --- the LampGroup ID.
  * **lampState** --- key value pairs --- the LampGroup State. The keys are the property names 
                                          in the [LampState interface](LampState-v1.xml) interface.
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


  * **lampGroupID** --- string --- the LampGroup ID.

#### PulseLampGroupWithState (lampGroupID, fromLampState, toLampState, period, duration, numPulses) -> (responseCode, lampGroupID)

This method allows the LSF Controller Clients to apply a Pulse Effect on a LampGroup. 

Input arguments:

  * **lampGroupID** --- string --- the LampGroup ID.
  * **fromLampState** --- key value pairs --- the fromState of the Pulse Effect. The keys are the property names 
                                              in the [LampState interface](LampState-v1.xml) interface.
  * **toLampState** --- key value pairs --- the toState of the Pulse Effect. The keys are the property names 
                                            in the [LampState interface](LampState-v1.xml) interface.
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


  * **lampGroupID** --- string --- the LampGroup ID.

#### PulseLampGroupWithPreset (lampGroupID, fromPresetID, toPresetID, period, duration, numPulses) -> (responseCode, lampGroupID)

This method allows the LSF Controller Clients to apply a Pulse Effect on a LampGroup. 

Input arguments:

  * **lampGroupID** --- string --- the LampGroup ID.
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


  * **lampGroupID** --- string --- the LampGroup ID.

#### TransitionLampGroupStateToPreset (lampGroupID, presetID, transitionPeriod) -> (responseCode, lampGroupID)

This method allows the LSF Controller Clients to transition to the LampGroup State specified by the presetID. 

Input arguments:

  * **lampGroupID** --- string --- the LampGroup ID.
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


  * **lampGroupID** --- string --- the LampGroup ID.

#### TransitionLampGroupStateField (lampGroupID, lampGroupStateFieldName, lampGroupStateFieldValue, transitionPeriod) -> (responseCode, lampGroupID, lampGroupStateFieldName)

This method allows the LSF Controller Clients to fetch one of the state fields of a LampGroup. 

Input arguments:

  * **lampGroupID** --- string --- the LampGroup ID.
  * **lampGroupStateFieldName** --- string --- the name of the LampGroup State field.
  * **lampGroupStateFieldValue** --- variant --- the value of the LampGroup State field.
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


  * **lampGroupID** --- string --- the LampGroup ID.
  * **lampGroupStateFieldName** --- string --- the name of the LampGroup State field.

#### ResetLampGroupState (lampGroupID) -> (responseCode, lampGroupID)

This method allows the LSF Controller Clients to reset the LampGroup State. 

Input arguments:

  * **lampGroupID** --- string --- the LampGroup ID.

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


  * **lampGroupID** --- string --- the LampGroup ID.

#### ResetLampGroupStateField (lampGroupID, lampGroupStateFieldName) -> (responseCode, lampGroupID, lampGroupStateFieldName)

This method allows the LSF Controller Clients to reset a LampGroup State field. 

Input arguments:

  * **lampGroupID** --- string --- the LampGroup ID.
  * **lampGroupStateFieldName** --- string --- the name of the LampGroup State field.

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


  * **lampGroupID** --- string --- the LampGroup ID.
  * **lampGroupStateFieldName** --- string --- the name of the LampGroup State field.

### Signals

#### LampGroupsNameChanged -> (lampGroupIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The LampGroupsNameChanged signal is used to notify the list of the LampGroup IDs whose
names have changed.

Output arguments:

  * **lampGroupIDs** --- string[] --- the list of LampGroup IDs.

#### LampGroupsCreated -> (lampGroupIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The LampGroupsCreated signal is used to notify the list of the LampGroup IDs that have 
been created.

Output arguments:

  * **lampGroupIDs** --- string[] --- the list of LampGroup IDs that have been created.

#### LampGroupsUpdated -> (lampGroupIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The LampGroupsUpdated signal is used to notify the list of the LampGroup IDs that have 
been updated.

Output arguments:

  * **lampGroupIDs** --- string[] --- the list of LampGroup IDs that have been updated.

#### LampGroupsDeleted -> (lampGroupIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The LampGroupsDeleted signal is used to notify the list of the LampGroup IDs that have 
been deleted.

Output arguments:

  * **lampGroupIDs** --- string[] --- the list of LampGroup IDs that have been deleted.

## References

  * The XML definition of the [LampGroup interface](LampGroup-v1.xml)


