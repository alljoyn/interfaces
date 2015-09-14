# org.allseen.LSF.ControllerService.TransitionEffect version 1


## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to create, update, delete, lookup and apply Transition Effects on Lamps.

A Transition Effect transitions a Lamp or Lamp Group from it’s current state to a 
target state over a specified number of milliseconds.   For a Hue, Saturation, 
Color Temperature or Brightness, the transition is linear in the HSV color model.  
  
The Controller Service can store only a maximum of 100 Transition Effects or until the size of the 
persisted Transition Effects file reaches 127KB whichever occurs first. If the user attempts to create more 
than 100 Transition Effects, "no slot for new entry" is returned as the responseCode or if the user tries to
create a new Transition Effect after the size of the Transition Effect file reaches 127KB, "Not enough resources" is
returned as the responseCode.   

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

#### GetAllTransitionEffectIDs () -> (responseCode, transitionEffectIDs)

This method allows the LSF Controller Clients to fetch all the Transition Effect IDs. 

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

  * **transitionEffectIDs** --- string[] --- the list of all Transition Effect IDs.

#### GetTransitionEffectName (transitionEffectID, language) -> (responseCode, transitionEffectID, language, transitionEffectName)

This method allows the LSF Controller Clients to fetch the name of a Transition Effect specified by the transitionEffectID. 

Input arguments:

  * **transitionEffectID** --- string --- the Transition Effect ID.
  * **language** --- string --- the language tag associated with the Transition Effect Name.

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
| 16    | The entity of interest was not found                   			|

  * **transitionEffectID** --- string --- the Transition Effect ID.
  * **language** --- string --- the language tag associated with the Transition Effect Name.
  * **transitionEffectName** --- string --- the name of the Transition Effect. The max supported size is 256 bytes. 

#### SetTransitionEffectName (transitionEffectID, transitionEffectName, language) -> (responseCode, transitionEffectID, language)

This method allows the LSF Controller Clients to set the name of a Transition Effect specified by the transitionEffectID. 

Input arguments:

  * **transitionEffectID** --- string --- the Transition Effect ID.
  * **transitionEffectName** --- string --- the name of the Transition Effect. The max supported size is 256 bytes.
  * **language** --- string --- the language tag associated with the Transition Effect Name.

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
| 8     | Value provided was out of range                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 16    | The entity of interest was not found                   			|


  * **transitionEffectID** --- string --- the Transition Effect ID.
  * **language** --- string --- the language tag associated with the Transition Effect Name. 

#### CreateTransitionEffect (lampState, presetID, transitionPeriod, transitionEffectName, language) -> (responseCode, transitionEffectID)

This method allows the LSF Controller Clients to create a Transition Effect. 

Input arguments:

  * **lampState** --- key value pairs --- the state of the Transition Effect.
  * **presetID** --- string --- the state of the Transition Effect specified using a Preset ID.
  * **transitionPeriod** --- uint16 --- the Transition period in milliseconds.
  * **transitionEffectName** --- string --- the name of the Transition Effect. The max supported size is 256 bytes.
  * **language** --- string --- the language tag associated with the Transition Effect Name.

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
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 17    | There is no slot for new entry                                              	|

  * **transitionEffectID** --- string --- the ID of the created Transition Effect.

#### UpdateTransitionEffect (transitionEffectID, lampState, presetID, transitionPeriod) -> (responseCode, transitionEffectID)

This method allows the LSF Controller Clients to update a Transition Effect. 

Input arguments:

  * **transitionEffectID** --- string --- the ID of the Transition Effect.
  * **lampState** --- key value pairs --- the state of the Transition Effect.
  * **presetID** --- string --- the state of the Transition Effect specified using a Preset ID.
  * **transitionPeriod** --- uint16 --- the Transition period in milliseconds.

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

  * **transitionEffectID** --- string --- the ID of the Transition Effect.

#### DeleteTransitionEffect (transitionEffectID) -> (responseCode, transitionEffectID)

This method allows the LSF Controller Clients to delete a Transition Effect. 

Input arguments:

  * **transitionEffectID** --- string --- the ID of the Transition Effect.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status                                                   		|
| 1     | Unexpected NULL pointer                                          		|
| 2     | An operation was unexpected at this time                          		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 16    | The entity of interest was not found                   			|
| 18    | There is a dependency of the entity for which a delete request was received   |

  * **transitionEffectID** --- string --- the ID of the Transition Effect.

#### GetTransitionEffect (transitionEffectID) -> (responseCode, transitionEffectID, lampState, presetID, transitionPeriod)

This method allows the LSF Controller Clients to fetch a Transition Effect. 

Input arguments:

  * **transitionEffectID** --- string --- the ID of the Transition Effect.

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
| 16    | The entity of interest was not found                   			|

  * **transitionEffectID** --- string --- the ID of the Transition Effect.
  * **lampState** --- key value pairs --- the state of the Transition Effect.
  * **presetID** --- string --- the state of the Transition Effect specified using a Preset ID.
  * **transitionPeriod** --- uint16 --- the Transition period in milliseconds.

#### ApplyTransitionEffectOnLamps (transitionEffectID, lampIDs) -> (responseCode, transitionEffectID, lampIDs)

This method allows the LSF Controller Clients to apply a Transition Effect on a set of Lamps.

Input arguments:

  * **transitionEffectID** --- string --- the ID of the Transition Effect.
  * **lampIDs** --- string[] --- the list of Lamp IDs.

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
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|

  * **transitionEffectID** --- string --- the ID of the Transition Effect.
  * **lampIDs** --- string[] --- the list of Lamp IDs.

#### ApplyTransitionEffectOnLampGroups (transitionEffectID, lampGroupIDs) -> (responseCode, transitionEffectID, lampGroupIDs)

This method allows the LSF Controller Clients to apply a Transition Effect on a set of LampGroups.

Input arguments:

  * **transitionEffectID** --- string --- the ID of the Transition Effect.
  * **lampGroupIDs** --- string[] --- the list of LampGroup IDs.

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
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|

  * **transitionEffectID** --- string --- the ID of the Transition Effect.
  * **lampGroupIDs** --- string[] --- the list of LampGroup IDs.

### Signals

#### TransitionEffectsNameChanged -> (transitionEffectIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The TransitionEffectsNameChanged signal is used to notify the list of the Transition Effect IDs whose
names have changed.

Output arguments:

  * **transitionEffectIDs** --- string[] --- the list of Transition Effect IDs.

#### TransitionEffectsCreated -> (transitionEffectIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The TransitionEffectsCreated signal is used to notify the list of the Transition Effect IDs that have 
been created.

Output arguments:

  * **transitionEffectIDs** --- string[] --- the list of Transition Effect IDs that have been created.

#### TransitionEffectsUpdated -> (transitionEffectIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The TransitionEffectsUpdated signal is used to notify the list of the Transition Effect IDs that have 
been updated.

Output arguments:

  * **transitionEffectIDs** --- string[] --- the list of Transition Effect IDs that have been updated.

#### TransitionEffectsDeleted -> (transitionEffectIDs)

|                       |                                   |
|-----------------------|-----------------------------------|
| Signal Type           | sessioncast                       |

The TransitionEffectsDeleted signal is used to notify the list of the Transition Effect IDs that have 
been deleted.

Output arguments:

  * **transitionEffectIDs** --- string[] --- the list of Transition Effect IDs that have been deleted.

## References

  * The XML definition of the [TransitionEffect interface](TransitionEffect-v1.xml)

