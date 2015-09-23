# org.allseen.LSF.ControllerService.Scene version 1


## Theory of Operation
This interface is provided by the LSF Controller Service to enable the LSF Controller
Clients to create, update, delete, lookup and apply Scenes on Lamps.

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

#### GetAllSceneIDs () -> (responseCode, sceneIDs)

This method allows the LSF Controller Clients to fetch all the Scene IDs. 

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

  * **sceneIDs** --- string[] --- the list of all Scene IDs.

#### GetSceneName (sceneID, language) -> (responseCode, sceneID, language, sceneName)

This method allows the LSF Controller Clients to fetch the name of a Scene specified by the sceneID. 

Input arguments:

  * **sceneID** --- string --- the Scene ID.
  * **language** --- string --- the language associated with the Scene Name.

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

  * **sceneID** --- string --- the Scene ID.
  * **language** --- string --- the language associated with the Scene Name.
  * **sceneName** --- string --- the name of the Scene. 

#### SetSceneName (sceneID, sceneName, language) -> (responseCode, sceneID, language)

This method allows the LSF Controller Clients to set the name of a Scene specified by the sceneID. 

Input arguments:

  * **sceneID** --- string --- the Scene ID.
  * **sceneName** --- string --- the name of the Scene.
  * **language** --- string --- the language associated with the Scene Name.

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


  * **sceneID** --- string --- the Scene ID.
  * **language** --- string --- the language associated with the Scene Name. 

#### CreateScene (transitionlampsLampGroupsToState, transitionlampsLampGroupsToPreset, pulselampsLampGroupsWithState, pulselampsLampGroupsWithPreset, sceneName, language) -> (responseCode, sceneID)

This method allows the LSF Controller Clients to create a Scene. 

Input arguments:

  * **transitionlampsLampGroupsToState** --- array of struct[string[], string[], key-value pairs, uint16] --- Transition to State component.
  * **transitionlampsLampGroupsToPreset** --- array of struct[string[], string[], string, uint16] --- Transition to Preset component.
  * **pulselampsLampGroupsWithState** --- array of struct[string[], string[], key-value pairs, key-value pairs, uint16, uint16, uint16] --- Pulse with State component.
  * **pulselampsLampGroupsWithPreset** --- array of struct[string[], string[], string, string, uint16, uint16, uint16] --- Pulse with Preset component.
  * **sceneName** --- string --- the Scene Name.
  * **language** --- string --- the language associated with the Scene Name.

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

  * **sceneID** --- string --- the ID of the created Scene.

#### UpdateScene (sceneID, transitionlampsLampGroupsToState, transitionlampsLampGroupsToPreset, pulselampsLampGroupsWithState, pulselampsLampGroupsWithPreset) -> (responseCode, sceneID)

This method allows the LSF Controller Clients to update a Scene. 

Input arguments:

  * **sceneID** --- string --- the ID of the Scene.
  * **transitionlampsLampGroupsToState** --- array of struct[string[], string[], key-value pairs, uint16] --- Transition to State component.
  * **transitionlampsLampGroupsToPreset** --- array of struct[string[], string[], string, uint16] --- Transition to Preset component.
  * **pulselampsLampGroupsWithState** --- array of struct[string[], string[], key-value pairs, key-value pairs, uint16, uint16, uint16] --- Pulse with State component.
  * **pulselampsLampGroupsWithPreset** --- array of struct[string[], string[], string, string, uint16, uint16, uint16] --- Pulse with Preset component.

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

  * **sceneID** --- string --- the ID of the Scene.

#### DeleteScene (sceneID) -> (responseCode, sceneID)

This method allows the LSF Controller Clients to delete a Scene. 

Input arguments:

  * **sceneID** --- string --- the ID of the Scene.

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

  * **sceneID** --- string --- the ID of the Scene.

#### GetScene (sceneID) -> (responseCode, sceneID, transitionlampsLampGroupsToState, transitionlampsLampGroupsToPreset, pulselampsLampGroupsWithState, pulselampsLampGroupsWithPreset)

This method allows the LSF Controller Clients to fetch a Scene. 

Input arguments:

  * **sceneID** --- string --- the ID of the Scene.

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

  * **sceneID** --- string --- the ID of the Scene.
  * **transitionlampsLampGroupsToState** --- array of struct[string[], string[], key-value pairs, uint16] --- Transition to State component.
  * **transitionlampsLampGroupsToPreset** --- array of struct[string[], string[], string, uint16] --- Transition to Preset component.
  * **pulselampsLampGroupsWithState** --- array of struct[string[], string[], key-value pairs, key-value pairs, uint16, uint16, uint16] --- Pulse with State component.
  * **pulselampsLampGroupsWithPreset** --- array of struct[string[], string[], string, string, uint16, uint16, uint16] --- Pulse with Preset component.

#### ApplyScene (sceneID) -> (responseCode, sceneID)

This method allows the LSF Controller Clients to apply a Scene.

Input arguments:

  * **sceneID** --- string --- the ID of the Scene.

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

  * **sceneID** --- string --- the ID of the Scene.

### Signals

#### ScenesNameChanged -> (sceneIDs)

The ScenesNameChanged signal is used to notify the list of the Scene IDs whose
names have changed.

Output arguments:

  * **sceneIDs** --- string[] --- the list of Scene IDs.

#### ScenesCreated -> (sceneIDs)

The ScenesCreated signal is used to notify the list of the Scene IDs that have 
been created.

Output arguments:

  * **sceneIDs** --- string[] --- the list of Scene IDs that have been created.

#### ScenesUpdated -> (sceneIDs)

The ScenesUpdated signal is used to notify the list of the Scene IDs that have 
been updated.

Output arguments:

  * **sceneIDs** --- string[] --- the list of Scene IDs that have been updated.

#### ScenesDeleted -> (sceneIDs)

The ScenesDeleted signal is used to notify the list of the Scene IDs that have 
been deleted.

Output arguments:

  * **sceneIDs** --- string[] --- the list of Scene IDs that have been deleted.

#### ScenesApplied -> (sceneIDs)

The ScenesApplied signal is used to notify the list of the Scene IDs that have 
been applied.

Output arguments:

  * **sceneIDs** --- string[] --- the list of Scene IDs that have been applied.

## References

  * The XML definition of the [Scene interface](Scene-v1.xml)
  * The LSF HLD can be found at [the LSF wiki]()

