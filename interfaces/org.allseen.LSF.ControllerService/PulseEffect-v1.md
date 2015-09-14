# org.allseen.LSF.ControllerService.PulseEffect version 1


## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to create, update, delete, lookup and apply Pulse Effects on Lamps.

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

#### GetAllPulseEffectIDs () -> (responseCode, pulseEffectIDs)

This method allows the LSF Controller Clients to fetch all the Pulse Effect IDs. 

Input arguments: None

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

  * **pulseEffectIDs** --- string[] --- the list of all Pulse Effect IDs.

#### GetPulseEffectName (pulseEffectID, language) -> (responseCode, pulseEffectID, language, pulseEffectName)

This method allows the LSF Controller Clients to fetch the name of a Pulse Effect specified by the pulseEffectID. 

Input arguments:

  * **pulseEffectID** --- string --- the Pulse Effect ID.
  * **language** --- string --- the language associated with the Pulse Effect Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

  * **pulseEffectID** --- string --- the Pulse Effect ID.
  * **language** --- string --- the language associated with the Pulse Effect Name.
  * **pulseEffectName** --- string --- the name of the Pulse Effect. 

#### SetPulseEffectName (pulseEffectID, pulseEffectName, language) -> (responseCode, pulseEffectID, language)

This method allows the LSF Controller Clients to set the name of a Pulse Effect specified by the pulseEffectID. 

Input arguments:

  * **pulseEffectID** --- string --- the Pulse Effect ID.
  * **pulseEffectName** --- string --- the name of the Pulse Effect.
  * **language** --- string --- the language associated with the Pulse Effect Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|


  * **pulseEffectID** --- string --- the Pulse Effect ID.
  * **language** --- string --- the language associated with the Pulse Effect Name. 

#### CreatePulseEffect (toLampState, pulsePeriod, pulseDuration, numPulses, fromLampState, toPresetID, fromPresetID, pulseEffectName, language) -> (responseCode, pulseEffectID)

This method allows the LSF Controller Clients to create a Pulse Effect. 

Input arguments:

  * **toLampState** --- key value pairs --- the toState of the Pulse Effect.
  * **pulsePeriod** --- uint16 --- the Pulse period.
  * **pulseDuration** --- uint16 --- the Pulse duration.
  * **numPulses** --- uint16 --- the number of Pulses.
  * **fromLampState** --- key value pairs --- the fromState of the Pulse Effect.
  * **toPresetID** --- string --- the toState of the Pulse Effect specified using a Preset ID.
  * **fromPresetID** --- string --- the fromState of the Pulse Effect specified using a Preset ID.
  * **pulseEffectName** --- string --- the name of the Pulse Effect.
  * **language** --- string --- the language associated with the Pulse Effect Name.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

  * **pulseEffectID** --- string --- the ID of the created Pulse Effect.

#### UpdatePulseEffect (pulseEffectID, toLampState, pulsePeriod, pulseDuration, numPulses, fromLampState, toPresetID, fromPresetID) -> (responseCode, pulseEffectID)

This method allows the LSF Controller Clients to update a Pulse Effect. 

Input arguments:

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.
  * **toLampState** --- key value pairs --- the toState of the Pulse Effect.
  * **pulsePeriod** --- uint16 --- the Pulse period.
  * **pulseDuration** --- uint16 --- the Pulse duration.
  * **numPulses** --- uint16 --- the number of Pulses.
  * **fromLampState** --- key value pairs --- the fromState of the Pulse Effect.
  * **toPresetID** --- string --- the toState of the Pulse Effect specified using a Preset ID.
  * **fromPresetID** --- string --- the fromState of the Pulse Effect specified using a Preset ID.

Output arguments:

  * **responseCode** --- uint16 --- the response code indicating the status of the operation. The list of valid
    values are:

| Value | Description                                                       		|
|-------|-------------------------------------------------------------------------------|
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

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
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

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
| 0     | Success status.                                                   		|
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.
  * **toLampState** --- key value pairs --- the toState of the Pulse Effect.
  * **pulsePeriod** --- uint16 --- the Pulse period.
  * **pulseDuration** --- uint16 --- the Pulse duration.
  * **numPulses** --- uint16 --- the number of Pulses.
  * **fromLampState** --- key value pairs --- the fromState of the Pulse Effect.
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
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

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
| 1     | Unexpected NULL pointer.                                          		|
| 2     | An operation was unexpected at this time                          		|
| 3     | A value was invalid                                               		|
| 4     | A unknown value                                                   		|
| 5     | A failure has occurred                                            		|
| 6     | An operation failed and should be retried later                   		|
| 7     | The request was rejected                                          		|
| 8     | Value provided was out of range                                   		|
| 9     | Invalid param/state field                                         		|
| 10    | Invalid message                                                   		|
| 11    | The arguments were invalid                                        		|
| 12    | The name is empty                          					|
| 13    | Not enough resources                                               		|
| 14    | The reply received for a message had invalid arguments                        |
| 15    | The requested operation was only partially successful                         |
| 16    | The entity of interest was not found                   			|
| 17    | There is no slot for new entry                                              	|
| 18    | There is a dependency of the entity for which a delete request was received   |
| 19    | The last LSF response code                                         		|

  * **pulseEffectID** --- string --- the ID of the Pulse Effect.
  * **lampGroupIDs** --- string[] --- the list of LampGroup IDs.

### Signals

#### PulseEffectsNameChanged -> (pulseEffectIDs)

The PulseEffectsNameChanged signal is used to notify the list of the Pulse Effect IDs whose
names have changed.

Output arguments:

  * **pulseEffectIDs** --- string[] --- the list of Pulse Effect IDs.

#### PulseEffectsCreated -> (pulseEffectIDs)

The PulseEffectsCreated signal is used to notify the list of the Pulse Effect IDs that have 
been created.

Output arguments:

  * **pulseEffectIDs** --- string[] --- the list of Pulse Effect IDs that have been created.

#### PulseEffectsUpdated -> (pulseEffectIDs)

The PulseEffectsUpdated signal is used to notify the list of the Pulse Effect IDs that have 
been updated.

Output arguments:

  * **pulseEffectIDs** --- string[] --- the list of Pulse Effect IDs that have been updated.

#### PulseEffectsDeleted -> (pulseEffectIDs)

The PulseEffectsDeleted signal is used to notify the list of the Pulse Effect IDs that have 
been deleted.

Output arguments:

  * **pulseEffectIDs** --- string[] --- the list of Pulse Effect IDs that have been deleted.

## References

  * The XML definition of the [PulseEffect interface](PulseEffect-v1.xml)
  * The LSF HLD can be found at [the LSF wiki]()

