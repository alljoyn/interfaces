# org.allseen.LSF.ControllerService.TransitionEffect version 1


## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to create, update, delete, lookup and apply Transition Effects on Lamps.

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

  * **transitionEffectID** --- string --- the Transition Effect ID.
  * **language** --- string --- the language tag associated with the Transition Effect Name.
  * **transitionEffectName** --- string --- the name of the Transition Effect. 

#### SetTransitionEffectName (transitionEffectID, transitionEffectName, language) -> (responseCode, transitionEffectID, language)

This method allows the LSF Controller Clients to set the name of a Transition Effect specified by the transitionEffectID. 

Input arguments:

  * **transitionEffectID** --- string --- the Transition Effect ID.
  * **transitionEffectName** --- string --- the name of the Transition Effect.
  * **language** --- string --- the language tag associated with the Transition Effect Name.

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


  * **transitionEffectID** --- string --- the Transition Effect ID.
  * **language** --- string --- the language tag associated with the Transition Effect Name. 

#### CreateTransitionEffect (lampState, presetID, transitionPeriod, transitionEffectName, language) -> (responseCode, transitionEffectID)

This method allows the LSF Controller Clients to create a Transition Effect. 

Input arguments:

  * **lampState** --- key value pairs --- the state of the Transition Effect.
  * **presetID** --- string --- the state of the Transition Effect specified using a Preset ID.
  * **transitionPeriod** --- uint16 --- the Transition period in milliseconds.
  * **transitionEffectName** --- string --- the name of the Transition Effect.
  * **language** --- string --- the language tag associated with the Transition Effect Name.

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

  * **transitionEffectID** --- string --- the ID of the Transition Effect.
  * **lampGroupIDs** --- string[] --- the list of LampGroup IDs.

### Signals

#### TransitionEffectsNameChanged -> (transitionEffectIDs)

The TransitionEffectsNameChanged signal is used to notify the list of the Transition Effect IDs whose
names have changed.

Output arguments:

  * **transitionEffectIDs** --- string[] --- the list of Transition Effect IDs.

#### TransitionEffectsCreated -> (transitionEffectIDs)

The TransitionEffectsCreated signal is used to notify the list of the Transition Effect IDs that have 
been created.

Output arguments:

  * **transitionEffectIDs** --- string[] --- the list of Transition Effect IDs that have been created.

#### TransitionEffectsUpdated -> (transitionEffectIDs)

The TransitionEffectsUpdated signal is used to notify the list of the Transition Effect IDs that have 
been updated.

Output arguments:

  * **transitionEffectIDs** --- string[] --- the list of Transition Effect IDs that have been updated.

#### TransitionEffectsDeleted -> (transitionEffectIDs)

The TransitionEffectsDeleted signal is used to notify the list of the Transition Effect IDs that have 
been deleted.

Output arguments:

  * **transitionEffectIDs** --- string[] --- the list of Transition Effect IDs that have been deleted.

## References

  * The XML definition of the [TransitionEffect interface](TransitionEffect-v1.xml)
  * The LSF HLD can be found at [the LSF wiki]()

